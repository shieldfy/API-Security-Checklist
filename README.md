(中文版请戳这:[中文版](./README-zh.md))
| (Português (Brasil): [README-pt_BR.md](./README-pt_BR.md))
| (française: [README-fr.md](./README-fr.md))
| (한국의: [README-ko.md](./README-ko.md))

# API Security Checklist
Checklist of the most important security countermeasures when designing, testing, and releasing your API.

------------------------------------------------------------------------------
## Authentication
- [ ] Don't use `Basic Auth` Use standard authentication (e.g. JWT, OAuth).
- [ ] Don't reinvent the wheel in `Authentication`, `token generating`, `password storing` use the standards.

### JWT (JSON Web Token)
- [ ] Use random complicated key (`JWT Secret`) to make brute forcing token very hard.
- [ ] Don't extract the algorithm from the payload. Force algorithm in the backend (`HS256` or `RS256`).
- [ ] Make token expiration (`TTL`, `RTTL`) as short as possible.
- [ ] Don't store sensitive data in the JWT payload, it can be decoded [easily](https://jwt.io/#debugger-io).

### OAuth
- [ ] Always validate `redirect_uri` on server side to allow only whitelisted URLs.
- [ ] Always try to exchange for code not tokens (don't allow `response_type=token`).
- [ ] Use `state` parameter with a random hash to prevent CSRF on OAuth authentication process.
- [ ] Define default scope, and validate scope parameter for each application.

## Access
- [ ] Limit requests (Throttling) to avoid DDoS / Bruteforce attacks.
- [ ] Use HTTPS on server side to avoid MITM (Man In The Middle Attack).
- [ ] Use `HSTS` header with SSL to avoid SSL Strip attack.

## Input
- [ ] Use proper HTTP method according to operation, `GET (read)`, `POST (create)`, `PUT (replace/update)` and `DELETE (to delete a record)`.
- [ ] Validate `content-type` on request Accept header (Content Negotiation) to allow only your supported format (e.g. `application/xml`, `application/json` ... etc) and respond with `406 Not Acceptable` response if not matched.
- [ ] Validate `content-type` of posted data as you accept (e.g. `application/x-www-form-urlencoded`, `multipart/form-data ,application/json` ... etc ).
- [ ] Validate User input to avoid common vulnerabilities (e.g. `XSS`, `SQL-Injection`, `Remote Code Execution` ... etc).
- [ ] Don't use any sensitive data (`credentials`, `Passwords`, `security tokens`, or `API keys`) in the URL, but use standard Authorization header.

## Processing
- [ ] Check if all the endpoints are protected behind authentication to avoid broken authentication process.
- [ ] User own resource id should be avoided. Use `/me/orders` instead of `/user/654321/orders`
- [ ] Don't use auto increment id's use `UUID` instead.
- [ ] If you are parsing XML files, make sure entity parsing is not enabled to avoid `XXE` (XML external entity attack).
- [ ] If you are parsing XML files, make sure entity expansion is not enabled to avoid `Billion Laughs/XML bomb` via exponential entity expansion attack.
- [ ] Use CDN for file uploads.
- [ ] If you are dealing with huge amount of data, use Workers and Queues to return response fast to avoid HTTP Blocking.
- [ ] Do not forget to turn the DEBUG mode OFF.

## Output
- [ ] Send `X-Content-Type-Options: nosniff` header.
- [ ] Send `X-Frame-Options: deny` header.
- [ ] Send `Content-Security-Policy: default-src 'none'` header.
- [ ] Remove fingerprinting headers - `X-Powered-By`, `Server`, `X-AspNet-Version` etc.
- [ ] Force `content-type` for your response, if you return `application/json` then your response `content-type` is `application/json`.
- [ ] Don't return sensitive data like `credentials`, `Passwords`, `security tokens`.
- [ ] Return the proper status code according to the operation completed. (e.g. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed` ... etc).


------------------------------------------------------------------------------

# Contribution
Feel free to contribute by forking this repository, making some changes, and submitting pull requests. For any questions drop us an email at `team@shieldfy.io`.
