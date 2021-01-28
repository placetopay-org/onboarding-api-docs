# Identificaciones de prueba

OnBoarding cuenta con un mock pensando en despliegues para ambientes dispuestos a que los clientes configuren y validen la integración con OnBoarding, todo esto sin generar consumos reales.

El Mock de OnBoarding permite simular escenarios diferentes, no hay restricción en el correo electrónico o la linea telefónica o algún otro dato solicitado al usuario a excepción del de documento, ya que es este quien define el comportamiento que tendrá la aplicación en su validación de identidad, adicional a esto si se desea usar algún documento que permita desencadenar una validación exitosa se debe usar el nombre **Pedro Alberto** y el Apellido **Pérez Jiménez** al momento de llenar los campos en la petición de validación, esto si se quiere que la validación arroje un resultado que apruebe que la identidad fue reconocida y que no arrojo alertas; ya que este es el nombre que se espera en cualquier escenario y dentro de la validación se compara la igualdad de los nombres y apellidos ingresados con lo obtenidos en los servicios consultados. El mock ofrece una serie de documentos de identidad para simular cada flujo que se podría generar al momento de validar una identidad.

Documentos de identidad y respectivo flujo habilitados en el mock

-   **1061111110** Todos los servicios en estado completado y sin generar riesgo, además con generación de 2fa.

-   **1061111111** Todos los servicios en estado completado con coincidencia en listas, peps y noticiaspara el servicio de sarlaft, además con generación de 2fa.

-   **1061111112** Todos los servicios en estado completado, pero sin generar 2fa (los datos de contacto no coinciden).

-   **1061111113** Reservada para pruebas internas, por favor no usar.

-   **1061111114** Reservada para pruebas internas, por favor no usar.

-   **1061111115** Reservada para pruebas internas, por favor no usar.

-   **1061111116** Reservada para pruebas internas, por favor no usar.
    
-   **1061111118** Reservada para pruebas internas, por favor no usar.

-   **1061111119** Todos los servicios en estado completado, pero la identificaciòn proporcionada pertenece a una persona fallecida.

-   **1061111120** Todos los servicios en estado completado y sin generar riesgo, además con generación de 2fa únicamente mediante email, (ocurre un error en el servicio de SMS)

-   **1061111121** Todos los servicios en estado completado, pero la fecha de expedición proporcionada no es la correcta.

-   **1061111122** Todos los servicios en estado completado, pero la fecha de expedición proporcionada no es la correcta, además la identificaciòn proporcionada pertenece a una persona fallecida.

-   **1061111123** Todos los servicios en estado completado y sin generar riesgo, además con generación de 2fa únicamente mediante sms (el email de contacto no coincide).

-   **1061111124** Todos los servicios en estado completado y sin generar riesgo, además con generación de 2fa únicamente mediante email (el teléfono de contacto no coincide).

-   **1061111125** Todos los servicios en estado completado pero no se encontraron coincidencias en centrales de riesgo.

**Nota:** cualquier otro documento enviado tendrá el comportamiento del documento **1061111110** que es el comportamiento por defecto
