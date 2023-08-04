---
stoplight-id: n64fmc6pmmxuy
---

# Códigos de respuesta

La siguiente tabla lista los diferentes códigos que entrega el servicio en la propiedad `initial_status` al realizar una solicitud de validación de identidad:

| Descripción | Código |
| -------------------------------------------- | ------ |
| Ejecución con errores                         | 0      |
| Rechazado por lista de exclusión finalizada   | 1      |
| Coincide con TFA - Envío de correo electrónico | 2      |
| Coincide con TFA - Envío de mensaje móvil      | 3      |
| Coincide con todos los TFA - Enviados          | 4      |
| Sin coincidencia - Finalizado                  | 5      |
| Sin coincidencia con TFA - Finalizado          | 6      |
| Coincide con todos los TFA - Finalizado        | 7      |
| Coincide con TFA - Móvil confirmado - En cola de validaciones finalizadas | 8      |
| Coincide con TFA - Móvil fallido - Finalizado  | 9      |
| Coincide con TFA - Móvil bloqueado - Finalizado | 10     |
| Coincide con TFA - Móvil caducado - Finalizado | 11     |
| Coincide con TFA - Correo electrónico confirmado - En cola de validaciones finalizadas | 12     |
| Coincide con TFA - Correo electrónico fallido - Finalizado | 13     |
| Coincide con TFA - Correo electrónico bloqueado - Finalizado | 14     |
| Coincide con TFA - Correo electrónico caducado - Finalizado | 15     |
| Coincide con todos los TFA - Fallido - Finalizado | 16     |
| Coincide con todos los TFA - Bloqueado - Finalizado | 17     |
| Coincide con TFA - Correo electrónico reportado - Finalizado | 18     |
| Coincide con TFA - Móvil confirmado - Esperando validaciones en cola | 19     |
| Coincide con TFA - Correo electrónico confirmado - Esperando validaciones en cola | 20     |
| Coincide con riesgo - Documento - Finalizado   | 21     |
| Coincide con riesgo - Documento con problemas - Finalizado | 22     |
| SARLAFT - En cola                             | 37     |
| SARLAFT - Finalizado                          | 38     |


