# WebHook

Una vez finalizada la validación de identidad se realizará una notificación mediante una solicitud HTTPS POST, esta debe ser segura, debido a que se enviara información sensible de la persona a la cual se le está validando la identidad. 

Dentro de la notificación se encontraran la siguientes llaves:

#### request_id
el identificador de la petición a la cual se está respondiendo, el cual coincidirá con el retornado cuando se solicitó la validación de identidad.

#### allow_access
un valor booleano que indica si se considera que a la identidad validada es conveniente permitirle continuar dentro de la plataforma del comercio.

#### is_partial_response
un valor booleano que indica si esta respuesta es la definitiva o es una respuesta temporal; cuando se tiene todas las validaciones realizadas pero alguna de estas presenta fallos, entonces se envía un respuesta temporal para no retrasar procesos dentro de comercio y tan pronto se tenga solución al servicio fallido, se enví­a una nueva respuesta con todas las validaciones ejecutadas de manera satisfactoria, esto implica que el valor de la llave allow_access pueda cambiar.

#### score
valor numérico entre 0 y 100 que define la calificación obtenida al finalizar el proceso de validación de identidad.

#### summary
el resumen de los resultados obtenidos al realizar las distintas validaciones y se compone por varios grupos que representan cada una de las validaciones realizadas y el estado de estas.

#### signature
firma que permite validar que la fuente sea confiable. El token provisto al comercio se debe cifrar usando el algoritmo sha256 (la salida serán dígitos hexadecimales en minúsculas) y la salida debe coincidir con la firma enviada.

#### Ejemplo de la notificación

```
{
  "request_id": 3,
  "allow_access": true,
  "is_partial_response": false
  "score": 74.73,
  "summary": [
    {
      "service_name": "PERSONAL_INFORMATION",
      "service_status": "COMPLETED",
      "additional_data": {
          "generates_risk": false,
          "details": {
              "score": 74.73,
                  "details": {
                      "person": {
                          "name": {
                                "match": "WITH_SAFE_COINCIDENCE",
                                "score": 100
                          },
                          "age_range": null,
                      },
                      "status": "WITH_INFORMATION",
                      "document": {
                          "document_issue_date_match": true,
                          "document_with_valid_status": true
                      },
                      "contact_data": {
                          "email": {
                              "match": "WITH_SAFE_COINCIDENCE",
                              "score": 68.41
                          },
                          "mobile": {
                              "match": "WITH_SAFE_COINCIDENCE",
                              "score": 63.39
                          }
                      }
                  },
          }
      },
      "continue_if_fails": false
    },
    {
      "service_name": "SARLAFT",
      "service_status": "COMPLETED",
      "additional_data": {
          "details": {
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
          "details": {
              "email": {
                  "match": true,
                  "confirmation_status": "EXPIRED"
              },
              "mobile": {
                  "match": true,
                  "confirmation_status": "CONFIRMED"
              }
          },
          "generates_risk": false
      },
      "continue_if_fails": false
    }    
  ],
  "signature":
"224b5fadc653bd6361d2198090a49c15cfd6e416579cd9b5cd5048e78b842a0b"
}
```

## Respuesta:
Para confirmar la recepción de la respuesta, su terminal debe responder en formato json con un cuerpo que contiene una llave status y un valor RECIVED, ademas el código de estado HTTP debe estar en el rango de 2xx. Todos los códigos de respuesta fuera de este rango, incluidos los códigos 3xx, indicaran que no se recibió la respuesta.

Si no se recibe un código de estado HTTP 2xx, se repite el intento de notificación durante el transcurso del día, En caso de obtener una respuesta en el rango de los 4xx y 5xx se marca la notificación como fallida y deja de intentar enviarlo a su punto final y se envía un correo electrónico sobre el punto final mal configurado.

Debido a que es muy importante confirmar la recepción de la notificación de webhook, su punto final debe devolver un código de estado HTTP 2xx antes de cualquier lógica compleja que pueda causar un tiempo de espera.

#### Ejemplo de la respuesta

```
{
  "status": "RECEIVED",
}
```

##### Nota:
Al momento de la integración, el comercio debe dejar indicando la url donde se realizará la notificación.
