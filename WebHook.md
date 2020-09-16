# WebHook

Una vez finalizada la validación de identidad se realizará una notificación mediante una solicitud HTTPS POST, esta debe ser segura, debido a que se enviara información sensible de la persona a la cual se le está validando la identidad. Dentro de la respuesta se encontraran la siguientes llaves:

#### requet_id: 
el identificador de la petición a la cual se está respondiendo, el cual coincidirá con el retornado cuando se solicitó la validación de identidad.

#### allow_access:
un valor booleano que indicará si se considera que a la identidad validada es conveniente permitirle continuar dentro de la plataforma del comercio.

#### is_partial_response
un valor booleano que indicará si esta respuesta es la definitiva o es una respuesta temporal; cuando se tiene todas las validaciones realizadas pero alguna de estas presenta fallos, entonces se envía un respuesta temporal para no retrasar procesos dentro de comercio y tan pronto se dé solución al servicio fallido se envía una nueva respuesta con todas las validaciones ejecutadas de manera satisfactoria, esto implica que el valor de la llave allow_access pueda cambiar.

#### summary
el resumen de los resultados obtenidos al realizar las distintas validaciones y se compone por varios grupos que representan cada uno de las validaciones realizadas y el estado de estas.

#### signature
firma que permite validar que la fuente sea confiable. El token provisto al comercio se debe cifrar usando el algoritmo sha256 (la salida serán dígitos hexadecimales en minúsculas) y la salida debe coincidir con la firma enviada.



#### Ejemplo de la notificación

```
{
  "request_id": 3,
  "allow_access": true,
  "is_partial_response": true
  "summary": [
    {
      "service_name": "SARLAFT",
      "service_status": "FAILED",
      "continue_if_fails": true,
      "additional_data": {
        "generates_risk": null,
        "report_assets": {
          "list": null,
          "news": null,
          "peps": null
        }
      }
    },
    {
      "service_name": "DOCUMENT_VALIDATION",
      "service_status": "COMPLETED",
      "continue_if_fails": true,
      "additional_data": {
        "generates_risk": false,
        "status": ACTIVE
      }
    },
    {
      "service_name": "PERSONAL_INFORMATION",
      "service_status": "COMPLETED",
      "continue_if_fails": false,
      "additional_data": {
        "status": "WITH_INFORMATION",
        "generates_risk": false,
        "contact_details": {
          "name": {
            "percentage_match": 100
          },
          "email": {
            "match": false,
            "confirmation_status": null
          },
          "cellphone": {
            "match": true,
            "confirmation_status": "CONFIRMED"
          }
        }
      }
    }
  ],
  "signature":
"224b5fadc653bd6361d2198090a49c15cfd6e416579cd9b5cd5048e78b842a0b"
}
```

##### Nota:
Al momento de la integración el comercio debe dejar indicando la url donde se realizará la notificación.