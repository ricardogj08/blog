# Creando APIs RESTful con buenas prácticas

## Protocolo HTTP

El ***protocolo HTTP*** es una norma que describe como realizar una comunicación entre dos dispositivos para enviar y recibir información.

> El cliente envía una petición y el servidor entrega una respuesta.

Para realizar una petición a un servidor, se requiere del método de la petición (verbo), de dónde se va realizar la petición y la versión del protocolo HTTP:

`GET /index.html HTTP/1.1`

Al realizar una petición, el servidor entrega una respuesta con la versión del protocolo HTTP solicitada, el código de respuesta y una breve descripción del código de respuesta:

`HTTP/1.1 200 OK`

## APIs RESTful

*Las APIs RESTful (representational state tranfer) representan información.*

Características:

* Las APIs RESTful utilizan el protocolo HTTP para transferir información.
* Cada uno de los elementos de la web es un ***recurso*** y se representan en diferentes formatos (png, jpg, mp4, txt, pdf, xml, html, json, etc).
* Cada recurso se representa con un ***identificador único*** (URIs).
* Los recursos siempren deben ser accedidos a través de métodos HTTP estándar.
* Comunicaciones sin estado (al finalizar el proceso de comunicación, el servidor no sabe de donde se realizó la petición).
* Stateless (Cookies y JWT) - Son maneras de implementar comunicaciones con estado (el servidor sabe quien está realizando una petición).
* **Las APIs RESTful pueden responder en diferentes formatos para representar información** (texto plano, csv, xml, json).

## Métodos HTTP

* `GET` - Obtener información.
* `POST` - Crear información.
* `PUT` - Actualiza todo un recurso.
* `PATCH` - Actualizar parcialmente un recurso.
* `DELETE` - Borra un recurso.
* `HEAD` - Identifica si un recurso existe.
* `OPTIONS (CORS)` - Permite obtener permisos para realizar peticiones a otros dominios.

## Idempotencia

*Es la propiedad de cambiar o no cambiar información.* 

Cambia la información sobre el método:

* `POST`

No cambia la información sobre los métodos:

* `GET`
* `HEAD`
* `OPTIONS`
* `PUT`
* `PATCH`
* `DELETE`

## Códigos de respuesta

*Es la manera de indentificar la respuesta del servidor en base a la petición que se está realizando.*

* 1xx - Informativas.
* 2xx - Éxito.
* 3xx - Redirección.
* 4xx - Errores del cliente.
* 5xx - Errores del servidor.
* > 599 - Respuestas no oficiales del protocolo HTTP.

## JWT

*JSON Web Tokens, es un estándar para entregar credenciales de acceso.*

Partes:

* **Encabezado**: es un JSON que incluye el algoritmo por el cual fué firmado el token y el tipo de token.
* **Payload**: es un JSON que incluye datos o información que se quiere almacenar en un token.
* **Firma**: verifica si un token es válido y si pertenece al servidor que generó el token.

## Ejemplos de respuestas de una API

    "message": {
        type: "message",
        code: 200,
        description: "OK"
    },
    "data": {
        id: 8,
        name: "Ricardo",
        lastName: "García",
        age: 23
    }

    "message": {
        type: "error",
        code: 404,
        description: "No se encontró el usuario que está solicitando"
    },
    data: []

## Parámetros

*Se utilizan generalmente para enviar información que permita filtrar búsquedas.*

> Se representan por `clave = valor`

Por ejemplo:

`https://tudominio.com:8080/api/v1/users?limite=5&pagina=5`

`?limite=5` o `?limite=5&pagina=5`, todos los parámetros que se requieran se separan por el símbolo ampersand (`&`).

## CORS

*Es una protección que tienen los navegadores para que los usuarios no sean atacados con información proveniente de otros dominios diferentes al dominio de petición original.*

Desde el backend con el método `OPTIONS`,se define a que dominios el cliente solamente puede realizar peticiones a una API.

## HATEOAS

* Permiten identificar los recursos de una API sin necesidad de que el cliente conozca toda la documentación de una API RESTful.
* El cliente solo debe conocer la información del recurso principal y la navegación para otros recursos (se hace con la información que devuelve dicho recurso).
* Se utiliza con frecuencia en APIs públicas.

Por ejemplo:

    "message": {
        type: "message",
        code: 200,
        description: "OK"
    },
    "data": {
        id: 8,
        name: "Ricardo",
        lastName: "García",
        age: 23
    },
    "navegation": [
        {
            "description": "self",
            "link": "/api/v1/users/8"
        },
        {
            "description:" "resource",
            "link": "/api/v1/users"
        }
    ]

## Referencias

* [Protocolo HTTP.](https://es.wikipedia.org/wiki/Protocolo_de_transferencia_de_hipertexto)
* [Control de acceso HTTP (CORS).](https://developer.mozilla.org/es/docs/Web/HTTP/CORS)
* [Códigos de estado de respuesta HTTP.](https://developer.mozilla.org/es/docs/Web/HTTP/Status)
* <https://jwt.io/>
* ***Handler*** - Bloque de instrucciones de acción para una petición.
* ***Middleware*** - Bloque de instrucciones que se ejecuta antes o después de procesar una petición.
