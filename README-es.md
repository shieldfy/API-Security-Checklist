[English](./README.md) | [中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md)

# Lista de seguridad en APIs
Lista de las contramedidas de seguridad más importantes en cuanto al diseño, testing y publicación de tu API.


---

## Autenticación
- [ ] No uses `Basic Auth` Usa autenticación estándar (e.g. JWT, OAuth).
- [ ] No reinventes la rueda en `autenticación`, `generación de tokens`, `almacenamiento de contraseñas`. Usa los estándares.
- [ ] Usa políticas de límite de reintentos (`Max Retry`) y funcionalidades de jailing en el Login.
- [ ] Usa encriptación en toda la información que sea sensible.

### JWT (JSON Web Token)
- [ ] Usa claves aleatorias complejas (`JWT Secret`) para dificultar los ataques por fuerza bruta.
- [ ] No extraigas el algoritmo del contenido. Fuerza el algoritmo en el backend (`HS256` o `RS256`).
- [ ] Haz que la expiración del token (`TTL`, `RTTL`) sea tan corta como sea posible.
- [ ] No almacenes información sensible en el contenido del JWT, puede ser descodificado [fácilmente](https://jwt.io/#debugger-io).

### OAuth
- [ ] Siempre valida `redirect_uri` en el lado del servidor para permitir sólo ciertas URLs.
- [ ] Trata siempre de intercambiar código y no tokens (no permitas `response_type=token`).
- [ ] Usa el parámetro `state` con un hash aleatorio para prevenir CSRF en el proceso de autenticación OAuth.
- [ ] Define el ámbito (`scope`) por defecto, y valida los parámetros de ámbito para cada aplicación.

## Acceso
- [ ] Limita las peticiones (`Throttling`) para prevenir ataques DDoS y de fuerza bruta.
- [ ] Usa HTTPS en el lado del servidor para evitar ataques MITM (Man In The Middle Attack).
- [ ] Usa la cabecera `HSTS` con SSL para evitar SSL Strip attack.

## Entradas
- [ ] Usa el método HTTP apropiado a cada operación: `GET (lectura)`, `POST (creación)`, `PUT/PATCH (reemplazo/actualización)`, y `DELETE (borrado)`, y responde con `405 Method Not Allowed` si el método en la petición no es apropiado para el recurso.
- [ ] Valida el `content-type` en la cabecera `Accept` de las peticiones (Content Negotiation), para permitir sólo los formatos soportados (e.g. `application/xml`, `application/json`, etc) y responde con `406 Not Acceptable` si no hay coincidencias.
- [ ] Valida el `content-type` de información enviada en base a la que aceptes (e.g. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, etc).
- [ ] Valida las entradas que realizan los usuarios para evitar ataques comunes (e.g. `XSS`, `SQL-Injection`, `Remote Code Execution`, etc).
- [ ] No utilices información sensible (`credentials`, `Passwords`, `security tokens`, or `API keys`) en la URL, en su lugar usa la cabecera estándar `Authorization`.
- [ ] Usa un servicio de API Gateway para permitir almacenamiento en caché (caching), límite de peticiones (Rate Limit), Spike Arrest y el despliegue de APIs dinámicamente.

## Procesamiento
- [ ] Valida que todos los endpoints estén protegidos con autenticación para evitar romper el proceso de autenticación.
- [ ] Debes evitar los recursos bajo un ID de usuario. Usa `/me/orders` en lugar de `/user/654321/orders`.
- [ ] No uses IDs auto incrementales. Usa `UUID` en su lugar.
- [ ] Si estas procesando archivos XML, asegúrate de deshabilitar el procesamiento de entidades para evitar ataques `XXE` (XML external entity attack).
- [ ] Si estas procesando archivos XML, asegúrate de deshabilitar la expansión de entidades, para evitar un ataque `Billion Laughs/XML bomb` via expansión exponencial de entidades.
- [ ] Utiliza CDN para subidas de ficheros.
- [ ] Si lidias con grandes cantidades de información, utiliza Workers y Colas para procesar tanto cómo sea posible en segundo plano, y devuelve una respuesta rápido para evitar un bloqueo HTTP.
- [ ] No olvides deshabilitar el modo Debug.

## Salidas
- [ ] Envía la cabecera `X-Content-Type-Options: nosniff`.
- [ ] Envía la cabecera `X-Frame-Options: deny`.
- [ ] Envía la cabecera `Content-Security-Policy: default-src 'none'`.
- [ ] Elimina cabeceras que dejen huellas - `X-Powered-By`, `Server`, `X-AspNet-Version` etc.
- [ ] Fuerza `content-type` para tus respuestas, si devuelves un `json` entonces tu `content-type` es `application/json`.
- [ ] No devuelvas información sensible cómo `credenciales`, `contraseñas`, `tokens de seguridad`.
- [ ] Devuelve el código HTTP acorde a la operación completada. (e.g. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, etc).

## CI & CD
- [ ] Audita tu diseño e implementación con tests unitarios/integración y test coverage.
- [ ] Usa procesos de revisión de código y evita la auto aprobación.
- [ ] Asegura que todos los componentes de tus servicios se escanean estáticamente con un software AV antes de ir a producción, incluyendo librerías de terceros y dependencias.
- [ ] Diseña un proceso de `rollback` para tus `deploys`.


---

## Ver también:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Una colección de recursos útiles para la creación de APIs RESTful HTTP+JSON.


---

# Contribución
Siéntete libre de contribuir haciendo un fork de éste repositorio, haciendo cambios, y enviando pull requests. Para cualquier pregunta déjanos un email en `team@shieldfy.io`.
