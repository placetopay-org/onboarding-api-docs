# WebHook

Una vez finalizada la validación de identidad se realizará una notificación mediante una solicitud HTTPS POST, esta debe ser segura, debido a que se enviara información sensible de la persona a la cual se le está validando la identidad. 

Dentro de la notificación se encontraran la siguientes llaves:

#### request_id
el identificador de la petición a la cual se está respondiendo, el cual coincidirá con el retornado cuando se solicitó la validación de identidad.

#### allow_access
un valor booleano que indica si se considera que a la identidad validada es conveniente permitirle continuar dentro de la plataforma del comercio.

#### is_partial_response
un valor booleano que indica si esta respuesta es la definitiva o es una respuesta temporal; cuando se tiene casi todas las validaciones realizadas debido a que alguna de estas presenta fallos, entonces se envía un respuesta temporal para no retrasar procesos dentro de comercio y tan pronto se tenga solución al servicio fallido, se enví­a una nueva respuesta con todas las validaciones ejecutadas de manera satisfactoria, esto implica que el valor de la llave allow_access pueda cambiar (esto depende del valor de **continue_if_fails** para el servicio que presenta el fallo).

#### summary
el resumen de los resultados obtenidos al realizar las distintas validaciones y se compone por varios grupos que representan cada una de las validaciones realizadas y el estado de estas.

#### api_key
llave usada para realizar la autenticación al momento de solicitar la validación de identidad.

#### nonce
valor aleatorio usado para la generación de la firma.

#### signature
firma que permite validar que la fuente sea confiable. 

al momento de recibir la respuesta se debe genera un valor cifrado mediante una clave especificada usando el método HMAC y la salida debe coincidir con la firma enviada.

la clave debe ser el api-key con el cual se autentica al momento de solicitar la validación de identidad.

el algoritmo usado para el cifrado debe ser SHA256 y el mensaje cifrado es la concatenación del metdo http usado en la notificación + la url del ewbhook + el valor de la llave nonce

##### Ejemplo del armado del mensaje

"POST https://webhook.site/8cdb1045-0ca8-46a9-a2bf-b9549afd8d8a 118617bc-8f9e-4a29-a91a-b773395919f0"

se debe tener presente que se deja un espacio entre cada parte del mensaje.


#### Ejemplo de la notificación

```
{
  "request_id": 3,
  "allow_access": true,
  "is_partial_response": false
  "summary": [
    {
      "service_name": "PERSONAL_INFORMATION",
      "service_status": "COMPLETED",
      "continue_if_fails": false,
      "additional_data": {
          "generates_risk": false,
          "details": {
              "score": 74.73,
                  "details": {
                      "status": "WITH_INFORMATION",
                      "person": {
                          "name": {
                                "match": "WITH_SAFE_COINCIDENCE",
                                "score": 100
                          },
                          "age_range": null,
                      },
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
      }
    },
    {
      "service_name": "SARLAFT",
      "service_status": "COMPLETED",
      "continue_if_fails": false,
      "generates_risk": false,
      "additional_data": {
          "details": {
              "list": false,
              "news": false,
              "peps": false,
              "news_score": 100,
              "peps_score": 100,
              "lists_score": 100,
          },
      }
    },
    {
      "service_name": "TFA",
      "service_status": "COMPLETED",
      "continue_if_fails": false,
      "additional_data": {
          "generates_risk": false,
          "details": {
              "email": {
                  "match": true,
                  "confirmation_status": "EXPIRED"
              },
              "mobile": {
                  "match": true,
                  "confirmation_status": "CONFIRMED"
              }
          }
      }
    }    
  ],
  "api_key": "JvG5kn0ktoIit49I",
  "nonce": "118617bc-8f9e-4a29-a91a-b773395919f0",
  "signature":
"506deb060f8d3d0c059b1285437fdd48edd65ec67b549ce5ffdec84e65cad822"
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
