---
tags: utilities, web, frontend, backend
---
----

[Documentación]()

----

# ¿Qué es HTTP?


# ¿Qué es un método HTTP?



# ¿Qué es un código de estado HTTP?



## 20X - Respuestas exitosas

### 200 - Success
Esta respuesta indica que la solicitud fue procesada de manera exitosa, esto en función del método HTTP utilizado:
- `GET`: - The resource has been fetched and transmitted in the message body.
- `HEAD`: The representation headers are included in the response without any message body.
- `PUT` or `POST`: The resource describing the result of the action is transmitted in the message body.
- `TRACE`: The message body contains the request message as received by the server.

### 201 - New Resource Created
The request succeeded, and a new resource was created as a result. This is typically the response sent after `POST` requests, or some `PUT` requests.

### 203 Non-Authoritative Information
This response code means the returned metadata is not exactly the same as is available from the origin server, but is collected from a local or a third-party copy. This is mostly used for mirrors or backups of another resource. Except for that specific case, the `200 OK` response is preferred to this status.

### 204 No Content
There is no content to send for this request, but the headers may be useful. The user agent may update its cached headers for this resource with the new ones.

### 205 Reset Content
Tells the user agent to reset the document which sent this request.

### 206 Partial Content
This response code is used when the Range header is sent from the client to request only part of a resource.


## 30X - Mensajes de redirección


## 40X - Errores del cliente

### 400 Bad Request
The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing).

### 401 Unauthorized
Although the HTTP standard specifies "unauthorized", semantically this response means "unauthenticated". That is, the client must authenticate itself to get the requested response.

### 402 Payment Required
This response code is reserved for future use. The initial aim for creating this code was using it for digital payment systems, however this status code is used very rarely and no standard convention exists.

### 403 Forbidden
The client does not have access rights to the content; that is, it is unauthorized, so the server is refusing to give the requested resource. Unlike `401 Unauthorized`, the client's identity is known to the server.

### 404 Not Found
The server cannot find the requested resource. In the browser, this means the URL is not recognized. In an API, this can also mean that the endpoint is valid but the resource itself does not exist. Servers may also send this response instead of `403 Forbidden` to hide the existence of a resource from an unauthorized client. This response code is probably the most well known due to its frequent occurrence on the web.

### 405 Method Not Allowed
The request method is known by the server but is not supported by the target resource. For example, an API may not allow calling `DELETE` to remove a resource.


## 50X - Errores del servidor

### 500 Internal Server Error
The server has encountered a situation it does not know how to handle.

### 501 Not Implemented
The request method is not supported by the server and cannot be handled. The only methods that servers are required to support (and therefore that must not return this code) are `GET` and `HEAD`.

### 502 Bad Gateway
This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response.