---
icon: key
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

# Manejo de CAPTCHA

Discord utiliza CAPTCHAs para prevenir abusos al realizar acciones de alto riesgo como iniciar sesión, aceptar invitaciones y enviar solicitudes de amistad. Algunos endpoints siempre devolverán un CAPTCHA, mientras que otros solo lo harán si el usuario está realizando solicitudes sospechosas o ha ejecutado muchas acciones de alto riesgo en poco tiempo.

No se debe asumir que solo un subconjunto de endpoints devolverá un CAPTCHA. Siempre hay que estar preparado para manejar CAPTCHAs al hacer peticiones a la API de Discord usando una cuenta de usuario.

### Identificación de CAPTCHA <a href="#undefined" id="undefined"></a>

Cuando una solicitud es desafiada, el endpoint devolverá un error 400 "bad request" con una respuesta que se parece a una respuesta de error heredada:

**Estructura de respuesta CAPTCHA**

<sup>1</sup> Para hCaptcha, la clave de sitio es dinámica y no debe ser codificada estáticamente.

<sup>2</sup> Si este campo está presente, debe usarse en la solicitud de desafío o fallará el reto.

| Campo                     | Tipo               | Descripción                                                                                            |
| ------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------ |
| captcha\_key              | array stringstring | Los errores del servicio CAPTCHA.                                                                      |
| captcha\_service          | string             | El [servicio CAPTCHA](https://docs.discord.food/topics/captcha-handling#captcha-service) a utilizar.   |
| captcha\_sitekey 1        | ?string            | La clave de sitio CAPTCHA (usada por hCaptcha).                                                        |
| captcha\_session\_id?     | string             | El ID de sesión del CAPTCHA (utilizado por hCaptcha).                                                  |
| captcha\_rqdata? 2        | string             | Datos personalizados que deben enviarse en solicitudes de desafío (utilizado por hCaptcha Enterprise). |
| captcha\_rqtoken?         | string             | El token de solicitud de desafío CAPTCHA (usado por hCaptcha Enterprise).                              |
| should\_serve\_invisible? | boolean            | Indica si el desafío CAPTCHA debe ser invisible.                                                       |

**Servicio CAPTCHA**

<sup>1</sup> reCAPTCHA no se utiliza actualmente por Discord, pero podría ser usado en el futuro.

| Valor                 | Descripción          | Clave del sitio                        |
| --------------------- | -------------------- | -------------------------------------- |
| hcaptcha              | hCaptcha             | Dinámico                               |
| recaptcha 1           | reCAPTCHA            | 6Lef5iQTAAAKeIvIY-DeexoO3gj7ryl9rLMEnn |
| recaptcha\_enterprise | reCAPTCHA Enterprise | 6LeYqFcqAAAD6iZesmNgVulsO4PkpBdr6NVG6M |

**Ejemplo de respuesta CAPTCHA**

```json
{
  "captcha_key": ["invalid-input-response", "response-already-used-error"],
  "captcha_sitekey": "f5561ba9-8f1e-40ca-9b5b-a0b3f719ef34",
  "captcha_service": "hcaptcha"
}
```

{% hint style="info" %}
En algunos endpoints que previamente no devolvían CAPTCHAs, el array `captcha_key` podría contener mensajes de error para mostrar a clientes antiguos que asumieron incorrectamente la disponibilidad del CAPTCHA. Por ejemplo:

```json
{
  "captcha_key": ["You need to update your app to join this server."],
  "captcha_sitekey": "b2b02ab5-7dae-4d6f-830e-7b55634c888b",
  "captcha_service": "hcaptcha",
  "captcha_session_id": "0ad46c1c-98e2-45f4-a09c-ddbfde9a301c",
  "captcha_rqdata": "32ODySYSI08RDvfcOKNA25zs8hGNVeD75qXuvAiuXnhgGV1tEqL/M6jHj9NJhuN3843/3Xb0y4qL9X85UhNWMLmXgvqDdUEJSKv1NlqrGqmBhi8qC2W4nKMzYeEFpGPMTqqC12Gp8BCZ+SPn9Dw0g3c=SMTWOoJOQKEpSoHZ",
  "captcha_rqtoken": "Im4rRTZXUWRTNFhyWTFpZUpEbVM3YVl1bFd6dU0zNUt0VkhzMEVoTHJNcC9wSmVqT29kS1FJVFpYYVgrT0RNd254YjFVQUE9PXVnalltRDZtNEFKOW85VXMi.ZfYH3g.beZ-lIsxWGcY5J0VUzE_odCbh24"
}
```

Para identificar correctamente los CAPTCHAs, los clientes deben revisar si hay un estado 400 bad request y el campo `captcha_key`, sin depender de los errores específicos en el array.
{% endhint %}

### Resolución de CAPTCHAs <a href="#undefined" id="undefined"></a>

Cuando se devuelve un CAPTCHA, el cliente debe mostrar un desafío CAPTCHA al usuario. Cómo se realiza esto depende del servicio CAPTCHA usado. Consulta la documentación de [hCaptcha](https://docs.hcaptcha.com/) y [reCAPTCHA](https://developers.google.com/recaptcha/docs/display) para más información.

Después de que el usuario haya resuelto el CAPTCHA, la solicitud debe reintentarse insertando la solución del CAPTCHA en la cabecera `X-Captcha-Key`. Además, si el campo `captcha_session_id` está presente, debe insertarse en la cabecera `X-Captcha-Session-Id`. Si el campo `captcha_rqtoken` está presente, debe insertarse en la cabecera `X-Captcha-Rqtoken`.

Si la solución no es aceptada, el endpoint devolverá otro error 400 bad request con una respuesta similar a la original del CAPTCHA. Ten en cuenta que una solución válida puede ser rechazada si la puntuación de bot es muy alta o si los campos `captcha_rqdata`/`captcha_rqtoken` no se manejan correctamente.

{% hint style="info" %}
Anteriormente, las soluciones CAPTCHA se enviaban en el cuerpo JSON de la solicitud usando los campos `captcha_key` y `captcha_rqtoken`. Este comportamiento está obsoleto y no debe usarse.
{% endhint %}
