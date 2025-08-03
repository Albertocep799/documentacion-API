---
icon: flask
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: false
---

# Experimentos

Los experimentos son una forma de pruebas A/B que utiliza Discord tanto del lado cliente como del servidor en sus aplicaciones para mostrar diferentes experiencias o comportamientos a usuarios diferentes aleatoriamente y/o basados en la ubicación, versión del cliente, etc.

En esencia, las implementaciones (rollouts) son simplemente YAML introducido en el panel administrativo de Discord; sin embargo, no todos los datos son requeridos por los clientes. Por eso los experimentos que los clientes ven están minificados y resultan complejos de descifrar.

### Huella Digital <a href="#huella-digital" id="huella-digital"></a>

Incluso el sitio web de marketing utiliza tests A/B para atraer usuarios a la app. Por ello, la aplicación necesita una forma única de identificar a quien usa la web sin usar autenticación (para usuarios que inicialmente visitan la landing page), así que recurre a "fingerprints" o huellas digitales.

Estas no son las típicas fingerprints generadas por información del navegador; son snowflakes generados por peticiones no autenticadas a obtener asignaciones de experimento. Se espera que las fingerprints se envíen en la cabecera `X-Fingerprint` en todas las peticiones posteriores a la API hasta autenticarse, para rastrear las pruebas A/B y permitir el acceso a partes de experimentos limitadas por API.

Una fingerprint consiste en un snowflake y un valor criptográfico hasheado. Tiene este aspecto: `1084179945133187083.JQddgNMmwJPghoBtFmaH7jTmdsw`.

Al registrar una nueva cuenta, la fingerprint se pasa, y (si es válida) se utiliza como ID del usuario creado. Esto es para preservar los experimentos en el usuario que acaba de registrarse. Por tanto, el tiempo de creación de una cuenta teóricamente representa la primera vez que visitó la web de marketing de Discord.

### Implementaciones <a href="#undefined" id="undefined"></a>

Las implementaciones de experimentos se definen en poblaciones basadas en la posición de implementación del usuario o servidor y pueden tener filtros que limiten la disponibilidad de cada población.

#### **Ejemplo de implementación**

```yaml
2023-02_stage_boosting (1816004721)

### Treatment 1

Filters

Guild Features: [COMMUNITY]
Member Count Range: 1000 - null
Hash Range: Hash Key: 1816004721, Target: 10000

Position Ranges

5000-9500, 9500-10000

### Control

Filters

Guild Features: [COMMUNITY]
Member Count Range: 1000 - null

Position Ranges

0 - 10000

### None

Position Ranges

0 - 10000
```

#### **Tratamientos**

`Control` y `None` están representados en la API como los valores enteros `0` y `-1`, respectivamente. Como advertencia, hay instancias donde ciertos experimentos tienen tratamientos innecesarios etiquetados como `Control` indexados como `1`.

#### **Posiciones de implementación**

Una posición de implementación se calcula con el siguiente seudocódigo, donde `exp_name` es el nombre del experimento y `resource_id` es el ID de usuario, fingerprint o servidor. Esta posición se usa junto con las poblaciones y filtros para averiguar a qué bucket ha sido asignado el experimento simplemente comprobando en qué tratamiento entra la población.

```python
result = mmh3.hash('exp_name:resource_id', signed=False) % 10000
```

#### **Estructuras de datos** <a href="#undefined" id="undefined"></a>

Hay dos tipos de experimentos, experimentos de usuario y experimentos de servidor. Los metadatos y nombres legibles por humanos están disponibles en los clientes. En la API los objetos no traen estos datos, salvo los experimentos de servidor, que pueden tener el nombre para el hash.

La mayoría de los objetos siguientes están representados como arrays siguiendo el orden documentado en los campos.

{% tabs %}
{% tab title="Experimentos de usuario" %}
<h2 align="center">Experimentos de usuario</h2>

La información devuelta por la API sobre experimentos de usuario es muy limitada. En contraste con la variedad de datos de los rollouts de servidor, solo obtenemos el bucket asignado al usuario o la [huella digital](experimentos.md#undefined).

{% hint style="warning" %}
Los experimentos de usuario aplican los mismos filtros y funcionamiento que los de servidor, aunque solo se da el resultado calculado.
{% endhint %}

<h3 align="center"><strong>Estructura de experimento de usuario</strong></h3>

Este objeto es un array de los siguientes campos:

<sup>1</sup> El bucket para experimentos A/A test debe ser None (`-1`) salvo que haya sobrescritura para el recurso.

<sup>2</sup> La información de holdout está presente solo si hay bucket asignado. Si hay holdout y el bucket es None (`-1`), el experimento está deshabilitado por el holdout. Los clientes pueden omitir este campo y solo seguir el campo `population` como siempre.

|        Campo        |   Tipo   | Descripción                                                                                             |
| :-----------------: | :------: | ------------------------------------------------------------------------------------------------------- |
|         hash        |  integer | Hash Murmur3 de 32 bits sin signo del nombre del experimento.                                           |
|       revision      |  integer | Versión actual del rollout.                                                                             |
|        bucket       |  integer | El bucket experimental asignado al usuario o fingerprint.                                               |
|       override      |  integer | Indica si el usuario o fingerprint tiene sobrescritura para el experimento (`-1` falso, `0` verdadero). |
|      population     |  integer | El grupo interno de población al que pertenece el usuario o fingerprint.                                |
|     hash\_result    |  integer | La posición de implementación calculada a usar, priorizada a cálculos locales.                          |
|      aa\_mode 1     |  integer | Modo A/A testing, representado como booleano entero.                                                    |
|  trigger\_debugging |  integer | Indica si trigger debugging de análisis está activado (booleano entero).                                |
|   holdout\_name 2   |  ?string | Nombre legible de un experimento (formato `año-mes_nombre`) que deshabilita el experimento.             |
| holdout\_revision 2 | ?integer | Revisión del experimento holdout.                                                                       |
|  holdout\_bucket 2  | ?integer | Bucket experimental asignado para el experimento holdout.                                               |

<h3 align="center"><strong>Ejemplo de experimento de usuario</strong></h3>

```json
[826493636, 3, -1, -1, 0, 203, 0, 1, "2025-02_user_profile_editing", 2, 0]
```
{% endtab %}

{% tab title="Experimentos de servidor" %}
### Experimentos de servidor

La información aquí es más detallada porque el cliente debe calcular el bucket a asignar por cada servidor. Puede parecer difícil de parsear dada la cantidad de arrays, pero es sencillo.

En casos raros, los experimentos de servidor han controlado rollouts de grupos DM; en estos casos la API no cambia, solo los filtros que no aplican no se usan. Ejemplo: experimento `2025-04_gdm_bedazzling`.

### Estructura de experimento de servidor

Este objeto es un array de los siguientes campos:

<sup>1</sup> Usado para agrupar varios experimentos coordinados.

<sup>2</sup> Bucket para experimentos A/A test debe ser None (`-1`) salvo sobrescritura.

<sup>3</sup> Si hay holdout y el servidor está en el bucket holdout, el bucket de población será None (`-1`) deshabilitando el experimento, salvo sobrescrituras.

<table><thead><tr><th align="center" valign="middle">Campo</th><th align="center">Tipo</th><th>Descripción</th></tr></thead><tbody><tr><td align="center" valign="middle">hash</td><td align="center">integer</td><td>Hash Murmur3 de 32 bits sin signo del nombre del experimento.</td></tr><tr><td align="center" valign="middle">hash_key 1</td><td align="center">?string</td><td>Nombre legible de experimento (formato <code>año-mes_nombre</code>) para hash, prioriza al usado en cliente.</td></tr><tr><td align="center" valign="middle">revision</td><td align="center">integer</td><td>Revisión actual de la implementación.</td></tr><tr><td align="center" valign="middle">populations</td><td align="center">array[objeto población experimental]</td><td>Las poblaciones rollout del experimento.</td></tr><tr><td align="center" valign="middle">overrides 2</td><td align="center">array[objeto sobrescritura experimental]</td><td>Sobrescrituras específicas para el experimento.</td></tr><tr><td align="center" valign="middle">overrides_formatted 2</td><td align="center">array[array[objeto población experimental]]</td><td>Poblaciones de sobrescritura para el experimento.</td></tr><tr><td align="center" valign="middle">holdout_name 3</td><td align="center">?string</td><td>Nombre legible de experimento (formato <code>año-mes_nombre</code>) que lo deshabilita.</td></tr><tr><td align="center" valign="middle">holdout_bucket 3</td><td align="center">?integer</td><td>Bucket holdout que deshabilita el experimento.</td></tr><tr><td align="center" valign="middle">aa_mode 2</td><td align="center">integer</td><td>Modo A/A testing representado como booleano entero.</td></tr><tr><td align="center" valign="middle">trigger_debugging</td><td align="center">integer</td><td>Si trigger debugging de analíticas está activado (booleano entero).</td></tr></tbody></table>

<h3 align="center">Ejemplo experimento de servidor</h3>

```json
[
  1405831955,
  "2021-06_guild_role_subscriptions",
  0,
  [
    [
      [
        [
          -1,
          [
            {
              "s": 7200,
              "e": 10000
            }
          ]
        ],
        [
          1,
          [
            {
              "s": 0,
              "e": 7200
            }
          ]
        ]
      ],
      [
        [
          2294888943,
          [
            [2690752156, 1405831955],
            [1982804121, 10000]
          ]
        ]
      ]
    ]
  ],
  [],
  [
    [
      [
        [
          [
            1,
            [
              {
                "s": 0,
                "e": 10000
              }
            ]
          ]
        ],
        [[1604612045, [[1183251248, ["GUILD_ROLE_SUBSCRIPTIONS"]]]]]
      ]
    ]
  ],
  null,
  null,
  0,
  0
]
```
{% endtab %}
{% endtabs %}

#### **Objeto de población experimental**

Define los filtros y rangos de posición que debe cumplir una población.

#### **Estructura de objeto de población experimental**

Es un array de los siguientes campos:

| Campo   | Tipo                                           | Descripción                                                          |
| ------- | ---------------------------------------------- | -------------------------------------------------------------------- |
| ranges  | array\[objeto rango de población experimental] | Los rangos para esta población.                                      |
| filters | objeto de filtros de población experimental    | Los filtros que el recurso debe cumplir para entrar en la población. |

#### **Ejemplo población experimental**

```json
[
  [
    [
      -1,
      [
        {
          "s": 7200,
          "e": 10000
        }
      ]
    ]
  ],
  [
    [
      2294888943,
      [
        [2690752156, 1405831955],
        [1982804121, 10000]
      ]
    ]
  ]
]
```

#### **Objeto de rango de población experimental**

Si los filtros de la población son válidos y el rango incluye la [posición de rollout](https://docs.discord.food/topics/experiments#rollout-positions), el recurso es elegible para ese bucket.

#### **Estructura de objeto de rango de población experimental**

Este objeto se representa como un array de los siguientes campos:

| Campo   | Tipo                                             | Descripción                      |
| ------- | ------------------------------------------------ | -------------------------------- |
| bucket  | integer                                          | El bucket que otorga este rango. |
| rollout | array\[objeto rollout de población experimental] | El rango del rollout.            |

#### **Estructura rollout de población experimental**

| Campo | Tipo    | Descripción              |
| ----- | ------- | ------------------------ |
| s     | integer | El inicio de este rango. |
| e     | integer | El final de este rango.  |

#### **Ejemplo de rango de población experimental**

```json
[
  1,
  [
    {
      "s": 0,
      "e": 4750
    }
  ]
]
```

#### **Objeto de filtros de población experimental**

Define los filtros necesarios para ser elegible para los rangos. Todos los filtros deben cumplirse para que el recurso sea elegible al bucket.

Los filtros son un objeto representado como array de arrays. El primer elemento del array anidado es el key hash, el segundo el valor. El orden es siempre el documentado aquí.

#### **Estructura filtros poblacionales experimentales**

| Campo                        | Tipo                              | Descripción                                                                                      |
| ---------------------------- | --------------------------------- | ------------------------------------------------------------------------------------------------ |
| guild\_has\_feature?         | objeto filtro feature de servidor | Las [features del servidor](https://docs.discord.food/resources/guild#guild-features) elegibles. |
| guild\_id\_range?            | objeto filtro rango por ID        | Rango de IDs de recurso elegibles.                                                               |
| guild\_age\_range\_days? 1   | objeto filtro rango por días      | Rango de días de vida del servidor elegibles.                                                    |
| guild\_member\_count\_range? | objeto filtro rango por miembros  | Rango de cantidad de miembros elegibles.                                                         |
| guild\_ids?                  | objeto filtro por ID              | Lista de IDs de recurso elegibles.                                                               |
| guild\_hub\_types?           | objeto filtro tipos de hub        | Lista de tipos de hub elegibles.                                                                 |
| guild\_has\_vanity\_url?     | objeto filtro vanity url          | Si el servidor debe tener vanity url para ser elegible.                                          |
| guild\_in\_range\_by\_hash?  | objeto filtro por hash/rango      | Límites especiales de posición de implementarción.                                               |
|                              |                                   |                                                                                                  |

<sup>1</sup> La edad del servidor se determina por el ID. Consulta la [documentación de snowflake](../discord-api.md#formato-snowflake). Puede calcularse así:

```json
timestamp = ((resource_id >> 22) + 1420070400000) / 1000
guild_age = (time.time() - timestamp) / 86400
```

### Estructuras de filtros

{% tabs %}
{% tab title="Características de servidor" %}
| Campo           | Tipo            | Descripción                                                          |
| --------------- | --------------- | -------------------------------------------------------------------- |
| guild\_features | array \[string] | Las funcionalidades del servidor elegibles; con que tenga una basta. |
{% endtab %}

{% tab title="Rango experimental" %}
| Campo   | Tipo       | Descripción                                     |
| ------- | ---------- | ----------------------------------------------- |
| min\_id | ?snowflake | El mínimo exclusivo para este rango, si existe. |
| max\_id | ?snowflake | El máximo exclusivo para este rango, si existe. |
{% endtab %}

{% tab title="ID" %}
| Campo      | Tipo               | Descripción                                |
| ---------- | ------------------ | ------------------------------------------ |
| guild\_ids | array \[snowflake] | Lista de IDs elegibles para esa población. |
{% endtab %}

{% tab title="Tipo de HUB" %}
| Campo             | Tipo             | Descripción                                |
| ----------------- | ---------------- | ------------------------------------------ |
| guild\_hub\_types | array \[integer] | Tipos de hub elegibles para esa población. |
{% endtab %}

{% tab title="Enlace personalizado" %}
| Campo                   | Tipo    | Descripción                                                    |
| ----------------------- | ------- | -------------------------------------------------------------- |
| guild\_has\_vanity\_url | boolean | El requisito de tener enlace personalizado para esa población. |
{% endtab %}
{% endtabs %}

#### **Estructura filtro rango por hash**

{% hint style="info" %}
Este filtro se utiliza para limitar la posición de implementación (rollout) mediante una clave hash adicional. La posición de implementación calculada debe ser menor que el objetivo proporcionado. La posición de implementación puede calcularse utilizando el siguiente pseudocódigo, donde `hash_key` es la clave hash proporcionada y `resource_id` es el ID de la guild:

```python
hashed = mmh3.hash('hash_key:resource_id', signed=False)
if hashed > 0:
    # Double the hash
    hashed += hashed
else:
    # Unsigned right shift by 0
    hashed = (hashed % 0x100000000) >> 0
result = hashed % 10000
```
{% endhint %}

| Campo     | Tipo    | Descripción                                                           |
| --------- | ------- | --------------------------------------------------------------------- |
| hash\_key | integer | Hash Murmur3 de 32 bits sin signo usado para determinar elegibilidad. |
| target    | integer | Límite de posición para esta población.                               |

#### **Ejemplo filtros de población experimental**

```json
[
  [
    1604612045, // guild_has_feature
    [
      [
        1183251248, // guild_features
        ["ROLE_SUBSCRIPTIONS_ENABLED"]
      ]
    ]
  ]
]
```

#### **Objeto de sobrescritura de bucket experimental**

Una sobrescritura representa el ajuste manual por empleados de Discord para dar acceso temprano o concreto a un experimento.

#### **Estructura de sobrescritura de bucket experimental**

| Campo | Tipo               | Descripción                                       |
| ----- | ------------------ | ------------------------------------------------- |
| b     | integer            | Bucket asignado a estos recursos.                 |
| k     | array \[snowflake] | Recursos que se les concede acceso a este bucket. |

#### Ejemplo de Sobrescritura de Bucket

```json
{
  "b": 1,
  "k": ["882680660588904448", "882703776794959873", "859533785225494528", "859533828754505741"]
}
```

### Endpoints <a href="#undefined" id="undefined"></a>

#### Obtener asignaciones de experimento

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /experiments

Devuelve las [asignaciones de experimento de usuario](experimentos.md#experimentos-de-usuario) y opcionalmente los rollouts de servidor para el usuario o fingerprint.

{% hint style="info" %}
Los experimentos devueltos dependen de las propiedades del cliente.
{% endhint %}

{% hint style="warning" %}
Una fingerprint solo se genera si no hay autorización ni fingerprint en las cabeceras de petición. El límite es 3 fingerprints válidas por 2 minutos por IP. Las extra no serán válidas.
{% endhint %}

#### Parámetros de cadena de consulta

<sup>1</sup> Incluir este parámetro requiere [autenticación](../discord-api.md#authentication) válida.

| Campo                     | Tipo    | Descripción                                           |
| ------------------------- | ------- | ----------------------------------------------------- |
| with\_guild\_experiments? | boolean | Si devolver experimentos de servidor en la respuesta. |
| platform? 1               | string  | Si incluir experimentos para la plataforma dada.      |

#### **Plataforma de experimentos**

| Valor             | Descripción                   |
| ----------------- | ----------------------------- |
| DEVELOPER\_PORTAL | El portal de desarrolladores. |

#### **Cuerpo de respuesta**

| Campo               | Tipo                                                                                | Descripción                                                                           |
| ------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| fingerprint?        | string                                                                              | Una [fingerprint](experimentos.md#huella-digital) generada de la fecha y hora actual. |
| assignments         | array\[[objeto asignación de experimento](experimentos.md#experimentos-de-usuario)] | Asignaciones de experimento para ese usuario o fingerprint.                           |
| guild\_experiments? | array\[[objeto experimento de servidor](experimentos.md#experimentos-de-servidor)]  | Rollouts experimentales de servidor para el cliente asignar.                          |

#### Crear fingerprint

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/fingerprint

[`Obsoleto`](../discord-api.md#endpoint-obsoleto)

Genera una nueva [fingerprint](experimentos.md#huella-digital).

{% hint style="warning" %}
El límite es de 3 fingerprints válidas por 2 minutos por IP. Más allá de este límite, se devuelven pero no serán válidas.
{% endhint %}

#### Cuerpo de respuesta

| Campo       | Tipo   | Descripción                                        |
| ----------- | ------ | -------------------------------------------------- |
| fingerprint | string | La fingerprint generada de la fecha y hora actual. |
