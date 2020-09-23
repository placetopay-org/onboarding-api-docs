# WebHook

Una vez finalizada la validación de identidad se realizará una notificación mediante una solicitud HTTPS POST, esta debe ser segura, debido a que se enviara información sensible de la persona a la cual se le está validando la identidad. 

Dentro de la notificación se encontraran la siguientes llaves:

#### requet_id
el identificador de la petición a la cual se está respondiendo, el cual coincidirá con el retornado cuando se solicitó la validación de identidad.

#### allow_access
un valor booleano que indica si se considera que a la identidad validada es conveniente permitirle continuar dentro de la plataforma del comercio.

#### is_partial_response
un valor booleano que indica si esta respuesta es la definitiva o es una respuesta temporal; cuando se tiene todas las validaciones realizadas pero alguna de estas presenta fallos, entonces se envía un respuesta temporal para no retrasar procesos dentro de comercio y tan pronto se tenga solución al servicio fallido, se enví­a una nueva respuesta con todas las validaciones ejecutadas de manera satisfactoria, esto implica que el valor de la llave allow_access pueda cambiar.

#### score
valor numérico entre 0 y 100 que define la calificación obtenida al finalizar el proceso de validación de identidad.

#### summary
el resumen de los resultados obtenidos al realizar las distintas validaciones y se compone por varios grupos que representan cada uno de las validaciones realizadas y el estado de estas.

#### signature
firma que permite validar que la fuente sea confiable. El token provisto al comercio se debe cifrar usando el algoritmo sha256 (la salida serán dígitos hexadecimales en minúsculas) y la salida debe coincidir con la firma enviada.

#### Ejemplo de la notificación

```
{
  "request_id": 3,
  "allow_access": true,
  "is_partial_response": false
  "score": 84.59,
  "summary": [
    {
      "service_name": "PERSONAL_INFORMATION",
      "service_status": "COMPLETED",
      "additional_data": {
          "generates_risk": true,
          "contact_details": {
              "person": {
                  "age_range": null,
                  "score_name": 24
              },
              "status": "WITH_INFORMATION",
              "document": {
                  "document_status": 1,
                  "document_issue_date_match": true
              },
              "contact_data": {
                  "email": {
                      "match": true,
                      "score": 68.49
                  },
                  "mobile": {
                      "match": true,
                      "score": 63.5
                  }
              }
          }
      },
      "continue_if_fails": false
    },
    {
      "service_name": "SARLAFT",
      "service_status": "COMPLETED",
      "additional_data": {
          "report_assets": {
              "list": false,
              "news": false,
              "peps": false
          },
          "generates_risk": false
      },
      "continue_if_fails": false
    },
    {
      "service_name": "TFA",
      "service_status": "COMPLETED",
      "additional_data": {
          "detail": {
              "email": {
                  "match": true,
                  "confirmation_status": "EXPIRED"
              },
              "mobile": {
                  "match": true,
                  "confirmation_status": "EXPIRED"
              }
          },
          "generates_risk": true
      },
      "continue_if_fails": false
    }    
  ],
  "signature":
"224b5fadc653bd6361d2198090a49c15cfd6e416579cd9b5cd5048e78b842a0b"
}
```

##### Nota:
Al momento de la integración, el comercio debe dejar indicando la url donde se realizará la notificación.
