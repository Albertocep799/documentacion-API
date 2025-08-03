---
icon: list-check
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

# Distribución del cliente

Mientras que los clientes móviles pueden distribuirse a través de la mayoría de tiendas de aplicaciones del sistema, los clientes de escritorio requieren una solución personalizada para descargar y actualizar. Discord proporciona varias APIs para descargar clientes, sus módulos nativos, y mantener todo actualizado. Visita la página de [descargas](https://discord.com/download) para aprender más sobre los clientes disponibles actualmente.

#### Enlaces base de distribución

Estas URLs, proporcionadas por conveniencia, dan acceso a todas las descargas de clientes (y redirigen el acceso a las alojadas en CDN), pero NO se usan para las peticiones de API de abajo. Para esas, consulta la URL Base de API. Ten en cuenta que los enlaces de descarga obtenidos de la API pueden no usar siempre estas URLs.

> [https://dl.discordapp.net/> \
> https://dl-ptb.discordapp.net/> \
> https://dl-canary.discordapp.net/> \
> https://dl-development.discordapp.net/](https://dl.discordapp.net/https://dl-ptb.discordapp.net/https://dl-canary.discordapp.net/https://dl-development.discordapp.net/)

### Clientes <a href="#clientes" id="clientes"></a>

### Web

Discord ofrece un cliente web que puede usarse en un navegador. Este mismo cliente web también se usa en el cliente de escritorio junto con módulos nativos para proporcionar una experiencia más rica.

#### Canal de lanzamiento web

| Valor               | URL                                                              | Descripción                    |
| ------------------- | ---------------------------------------------------------------- | ------------------------------ |
| stable              | [https://discord.com/app](https://discord.com/app)               | Compilación estable.           |
| ptb <sup>1</sup>    | [https://ptb.discord.com/app](https://ptb.discord.com/app)       | Compilación de prueba pública. |
| canary <sup>1</sup> | [https://canary.discord.com/app](https://canary.discord.com/app) | Compilación de prueba alfa.    |

<sup>1</sup> Consulta el artículo del Centro de Ayuda para más información sobre los clientes de prueba de Discord.

### Escritorio

Discord ofrece clientes de escritorio Electron para Windows, macOS y Linux.

#### Windows

La aplicación de Windows usa una pila de instalación y actualización separada de las otras plataformas de escritorio. Consulta Get Latest Distributed Application Installer para más información sobre obtener el último instalador de aplicación, y Get Latest Distributed Application Manifest para más información sobre obtener las últimas actualizaciones de aplicación.

#### macOS

Consulta Get Latest Application Installer para más información sobre obtener el último instalador de aplicación, y Get Application Updates para más información sobre obtener las últimas actualizaciones de aplicación.

#### Linux

Consulta Get Latest Application Installer para más información sobre obtener el último instalador de aplicación, y Get Application Updates para más información sobre obtener las últimas actualizaciones de aplicación. Ten en cuenta que la aplicación de Linux no se actualiza automáticamente.

#### Canal de lanzamiento de escritorio

Los canales de lanzamiento de escritorio siguen los canales de lanzamiento web al renderizar el cliente. Sin embargo, el host de aplicación y los módulos nativos se actualizan por separado del cliente mismo.

| Valor         | Descripción                    |
| ------------- | ------------------------------ |
| stable        | Compilación estable.           |
| ptb 1         | Compilación de prueba pública. |
| canary 1      | Compilación de prueba alfa.    |
| development 2 | Compilación de desarrollo.     |

<sup>1</sup> Consulta el artículo del Centro de Ayuda para más información sobre los clientes de prueba de Discord.

<sup>2</sup> La compilación de desarrollo sigue el canal de lanzamiento web `canary` y no se recomienda su uso. Puede ser inestable o estar rota en cualquier momento.

#### Tipo de plataforma de escritorio

| Valor | Descripción |
| ----- | ----------- |
| win   | Windows.    |
| osx   | macOS.      |
| linux | Linux.      |

#### Tipo de arquitectura de escritorio

| Valor | Descripción                 |
| ----- | --------------------------- |
| x86   | Compilación x86 de 32 bits. |
| x64   | Compilación x86 de 64 bits. |
| arm64 | Compilación ARM de 64 bits. |

#### Formato de ejecutable de escritorio

| Valor  | Descripción                            |
| ------ | -------------------------------------- |
| deb    | Archivo de paquete de software Debian. |
| tar.gz | Archivo de almacén comprimido.         |

### Móvil

Discord mantiene clientes móviles estables y beta tanto para Android como para iOS.

#### Canal de lanzamiento de Android

{% hint style="warning" %}
Los clientes oficiales de Android realizan una verificación de versión mínima al iniciar. Si la versión instalada es inferior a la versión mínima, el cliente se negará a iniciar y pedirá al usuario que actualice.

Esta información de la versión mínima está disponible en `https://dl.discordapp.net/apps/android/versions.json`. La respuesta contiene un campo `discord_android_min_version`, que es una cadena que indica la versión mínima requerida.
{% endhint %}

<sup>1</sup> Consulta el artículo del Centro de Ayuda para más información sobre los clientes de prueba de Discord.

| Valor   | URL                                                                                                                    | Descripción                        |
| ------- | ---------------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| stable  | [https://play.google.com/store/apps/details?id=com.discord](https://play.google.com/store/apps/details?id=com.discord) | Compilación estable de aplicación. |
| beta 1  | [https://play.google.com/apps/testing/com.discord](https://play.google.com/apps/testing/com.discord)                   | Compilación beta de aplicación.    |
| alpha 1 | [https://groups.google.com/g/discord-android-alpha-testers](https://groups.google.com/g/discord-android-alpha-testers) | Compilación alfa de aplicación.    |

#### Canal de lanzamiento de iOS

<sup>1</sup> Consulta el artículo del Centro de Ayuda para más información sobre los clientes de prueba de Discord.

| Valor   | URL                                                                                                                                          | Descripción                           |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| stable  | [https://apps.apple.com/us/app/discord-talk-chat-hang-out/id985746746](https://apps.apple.com/us/app/discord-talk-chat-hang-out/id985746746) | Compilación estable de aplicación.    |
| beta 1  | [https://testflight.apple.com/join/gdE4pRzI](https://testflight.apple.com/join/gdE4pRzI)                                                     | Compilación beta de aplicación.       |
| alpha 1 | \<private>                                                                                                                                   | Compilaciones internas de aplicación. |

### Endpoints <a href="#endpoints" id="endpoints"></a>

#### Obtener último instalador de aplicación

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /download/{release\_channel}

`Obsoleto`

Redirige al último instalador de aplicación para el canal de lanzamiento proporcionado y la plataforma seleccionada.

{% hint style="warning" %}
Este endpoint está en proceso de ser dado de baja en favor del endpoint obtén la última versión distribuida del instalador de la aplicación.
{% endhint %}

{% hint style="info" %}
Un canal de lanzamiento especial de la aplicación móvil puede usarse para redirigir a la página de descarga de los clientes móviles.
{% endhint %}

#### Parámetros de cadena de consulta

<sup>1</sup> Solo aplicable a la plataforma Linux.

| Nombre    | Tipo   | Descripción                                                                     |
| --------- | ------ | ------------------------------------------------------------------------------- |
| platform  | string | La plataforma para la que obtener el instalador.                                |
| format? 1 | string | El formato de ejecutable para el que obtener el instalador (por defecto `deb`). |

#### Obtener actualizaciones de aplicación

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /updates/{release\_channel}

`Obsoleto`

Devuelve información sobre la última actualización de host de aplicación para el canal de lanzamiento proporcionado y la plataforma seleccionada.

{% hint style="warning" %}
Este endpoint está en proceso de ser dado de baja en favor del endpoint obtén la última versión distribuida del instalador de la aplicación.
{% endhint %}

#### Parámetros de cadena de consulta

| Nombre    | Tipo   | Descripción                                                                         |
| --------- | ------ | ----------------------------------------------------------------------------------- |
| platform? | string | La plataforma para la que obtener información de actualización (por defecto `osx`). |

#### Cuerpo de respuesta

<sup>1</sup> Solo se proporciona si las actualizaciones automáticas están disponibles para la plataforma seleccionada.

| Nombre    | Tipo              | Descripción                                 |
| --------- | ----------------- | ------------------------------------------- |
| name      | string            | La última versión de host.                  |
| pub\_date | ISO8601 timestamp | Cuándo se publicó la actualización.         |
| url? 1    | string            | La URL al instalador correspondiente.       |
| notes? 1  | string            | Cualquier nota extra para la actualización. |

#### Ejemplo de respuesta

```python
{
  "name": "0.0.75",
  "pub_date": "2023-07-05T17:16:10",
  "url": "https://dl-ptb.discordapp.net/apps/osx/0.0.75/DiscordPTB.zip",
  "notes": ""
}
```

#### Obtener versiones de módulos nativos

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /modules/{release\_channel}/versions.json

`Obsoleto`

Devuelve un mapeo de nombres de módulos a versiones enteras representando las versiones de módulos nativos encontradas para el canal de lanzamiento proporcionado y la plataforma seleccionada.

{% hint style="warning" %}
Este endpoint está en proceso de ser dado de baja en favor del endpoint **obtén el último manifesto de la aplicación distribuida**.
{% endhint %}

{% hint style="info" %}
Los módulos nativos tienen una versión única por versión del host. Este endpoint puede devolver un objeto vacío si no hay módulos nativos disponibles para los parámetros proporcionados (por ejemplo, si la versión del host proporcionada no existe).
{% endhint %}

#### Parámetros de cadena de consulta

| Nombre         | Tipo   | Descripción                                                                            |
| -------------- | ------ | -------------------------------------------------------------------------------------- |
| platform?      | string | La plataforma para la que obtener información de actualización (por defecto `osx`).    |
| host\_version? | string | La versión de host para la que obtener información de actualización (por defecto `0`). |

#### Ejemplo de respuesta

```python
{
  "discord_cloudsync": 1,
  "discord_desktop_core": 1,
  "discord_dispatch": 1,
  "discord_erlpack": 1,
  "discord_game_utils": 1,
  "discord_krisp": 1,
  "discord_modules": 1,
  "discord_rpc": 1,
  "discord_spellcheck": 1,
  "discord_utils": 1,
  "discord_voice": 1
}
```

#### Obtener módulo nativo

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /modules/{release\_channel}/{module\_name}/{module\_version}

`Obsoleto`

Redirige a un archivo ZIP del módulo nativo para el canal de lanzamiento, nombre de módulo y versión de módulo proporcionados, si se encuentra.

{% hint style="warning" %}
Este endpoint está en proceso de ser dado de baja en favor del endpoint **obtén el último manifesto de la aplicación distribuida**.
{% endhint %}

{% hint style="info" %}
Los módulos nativos tienen una versión única por versión del host.
{% endhint %}

#### Parámetros de cadena de consulta

| Nombre         | Tipo   | Descripción                                                                            |
| -------------- | ------ | -------------------------------------------------------------------------------------- |
| platform?      | string | La plataforma para la que obtener información de actualización (por defecto `osx`).    |
| host\_version? | string | La versión de host para la que obtener información de actualización (por defecto `0`). |

#### Obtener último instalador de aplicación distribuida

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /downloads/distributions/app/installers/latest

Redirige al último instalador de aplicación para la plataforma seleccionada.

{% hint style="warning" %}
Este endpoint es actualmente exclusivo para Windows.
{% endhint %}

#### Parámetros de cadena de consulta

| Nombre   | Tipo   | Descripción                                                |
| -------- | ------ | ---------------------------------------------------------- |
| channel  | string | El canal de lanzamiento para el que obtener el instalador. |
| platform | string | La plataforma para la que obtener el instalador.           |
| arch     | string | La arquitectura para la que obtener el instalador.         |

#### Obtener último manifiesto de aplicación distribuida

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /updates/distributions/app/manifests/latest

Devuelve información sobre las últimas actualizaciones de aplicación para la plataforma seleccionada.

{% hint style="warning" %}
Este endpoint es actualmente exclusivo para Windows y también está disponible en `https://updates.discord.com/distributions/app/manifests/latest`.
{% endhint %}

{% hint style="info" %}
Debido al almacenamiento en caché, las nuevas versiones del host pueden no estar disponibles para todos inmediatamente después de su lanzamiento.
{% endhint %}

{% hint style="info" %}
Los módulos nativos tienen una versión única por cada versión del host.
{% endhint %}

#### Parámetros de cadena de consulta

| Nombre             | Tipo   | Descripción                                                                         |
| ------------------ | ------ | ----------------------------------------------------------------------------------- |
| install\_id?       | string | Un UUID único generado por el cliente para la instalación actual.                   |
| channel            | string | El canal de lanzamiento para el que obtener el manifiesto.                          |
| platform           | string | La plataforma para la que obtener el manifiesto.                                    |
| arch               | string | La arquitectura para la que obtener el manifiesto.                                  |
| platform\_version? | string | La versión del sistema operativo del cliente (por ejemplo `10.0.19045` en Windows). |

#### Cuerpo de respuesta

<sup>1</sup> Este campo solo se proporciona a través del endpoint de actualizaciones rehosteado anterior, y requiere que `install_id` y `platform_version` estén establecidos y sean compatibles.

| Nombre               | Tipo                                    | Descripción                                                   |
| -------------------- | --------------------------------------- | ------------------------------------------------------------- |
| full                 | manifest package version object         | El paquete de host completo para la última versión de host.   |
| deltas               | array\[manifest package version object] | Los paquetes de host delta para versiones de host anteriores. |
| modules              | map\[string, manifest package object]   | Los módulos nativos disponibles para descargar/actualizar.    |
| required\_modules    | array\[string]                          | Los nombres de los módulos nativos que el cliente requiere.   |
| metadata\_version? 1 | ?integer                                | La versión de los metadatos del manifiesto.                   |

#### Estructura de paquete de manifiesto

| Nombre | Tipo                                    | Descripción                                         |
| ------ | --------------------------------------- | --------------------------------------------------- |
| full   | manifest package version object         | El paquete completo para la última versión de host. |
| deltas | array\[manifest package version object] | El paquete delta para versiones de host anteriores. |

#### Estructura de versión de paquete de manifiesto

| Nombre           | Tipo                              | Descripción                                                      |
| ---------------- | --------------------------------- | ---------------------------------------------------------------- |
| host\_version    | array\[integer, integer, integer] | La versión de host a la que apunta el paquete.                   |
| module\_version? | integer                           | La versión del módulo incluida en el paquete.                    |
| package\_sha256  | string                            | El hash SHA256 del archivo del paquete.                          |
| url              | string                            | La URL de descarga al tarball del paquete comprimido con Brotli. |

#### Ejemplo de respuesta

```python
{
  "modules": {
    "discord_overlay2": {
      "full": {
        "host_version": [1, 0, 9015],
        "module_version": 1,
        "package_sha256": "baa1196292f888c8a90413ea19201849c7a8b7be1a52f2d6b9a185e04ab1b49a",
        "url": "https://dl.discordapp.net/distro/app/stable/win/x86/1.0.9015/discord_overlay2/1/full.distro"
      },
      "deltas": [
        {
          "host_version": [1, 0, 9014],
          "module_version": 1,
          "package_sha256": "7634e584b90bb0315fff0b69dd19712c1acbb0687657548e698e5348dc59c824",
          "url": "https://dl.discordapp.net/distro/app/stable/win/x86/1.0.9015/discord_overlay2/1/from/1.0.9014/1"
        },
        {
          "host_version": [1, 0, 9013],
          "module_version": 2,
          "package_sha256": "60b2876b144d918cf8f1ba61110162782c9dc52def8d64b97222cd607989c211",
          "url": "https://dl.discordapp.net/distro/app/stable/win/x86/1.0.9015/discord_overlay2/1/from/1.0.9013/2"
        }
      ]
    }
  },
  "full": {
    "host_version": [1, 0, 9015],
    "package_sha256": "bde31e984e70465fcc9dc01241e3fd8bbb3f84cb49567b8b9930a6a7bc193b7b",
    "url": "https://dl.discordapp.net/distro/app/stable/win/x86/1.0.9015/full.distro"
  },
  "deltas": [
    {
      "host_version": [1, 0, 9014],
      "package_sha256": "48b8f905c7a40ca588e02db4b2903926bc62dbc9e3c9f38f8548882724fac6fa",
      "url": "https://dl.discordapp.net/distro/app/stable/win/x86/1.0.9015/from/1.0.9014"
    },
    {
      "host_version": [1, 0, 9013],
      "package_sha256": "357914897b025320fe139e3ddb9bc8b81c8d4747947026b83d0627f282d35aff",
      "url": "https://dl.discordapp.net/distro/app/stable/win/x86/1.0.9015/from/1.0.9013"
    }
  ],
  "required_modules": [
    "discord_desktop_core",
    "discord_erlpack",
    "discord_spellcheck",
    "discord_utils",
    "discord_voice"
  ]
}
```
