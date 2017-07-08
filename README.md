# API-Security-Checklist
Checklist of the most important security countermeasures when designing,testing, and releasing your API


# AUTHENTICATION : JWT (JSON WEB TOKEN)
[] Use random complicated key (JWT Secret).
[] Force algorithm in the backend (`HS256` or `RS256`). 
[] Make token expiration (TTL , RTTL) short as possible.
[] Don't store sensetive data in the JWT payload.

# AUTHENTICATION : OAUTH
[] Always validate redirect_uri on server side to allow only whitelisted urls.
[] Always try to exchange for code not tokens (don't allow response_type=token)
[] Use `state` parameter to prevent CSRF on OAuth authentication process.

# ACCESS
[] Limit requests (Throttling) to avoid DDoS / Bruteforce attacks.
[] Use HTTPS on server side to avoid MITM.
[] Use HSTS header with SSL to avoid SSL Strip attack.

# INPUT 
[] Use proper HTTP method according to operation , GET (read), POST (create), PUT (replace/update) and DELETE (to delete a record).
[] Validate content-type on request Accept header ( Content Negotiation ) to allow only your supported format (e.g. application/xml , application/json ... etc) and respond with 406 Not Acceptable response if not matched.
[] Validate content-type of posted data as you accept (e.g. application/x-www-form-urlencoded , multipart/form-data ,application/json ... etc )
[] Validate User input to avoid common vulnerabilities (e.g. XSS, SQLI , RCE ... etc).
[] Don't use any sensetive data ( credentials. Passwords, security tokens, or API keys) in the URL, but use standard Authorization header.

# PROCESSING
[] Check if all endpoint protected behind the authentication.
[] User own resource id should be avoided. Use `/me/orders` instead of `/user/654321/orders`
[] Don't use auto increment id's use UUID instead.
[] If you are parsing XML files , make sure entity parsing is not enable to avoid XXE.
[] Use CDN for file uploads.
[] If you are dealing with huge amount of data , use Workers and Queues to return response fast to avoid HTTP Blocking. 
[] Do not forget and leave the DEBUG mode on.


# OUTPUT
[] Send X-Content-Type-Options: nosniff header.
[] Send X-Frame-Options: deny header.
[] Force content-type for your response , if you return application/json then your response content-type is application/json.
[] Don't return sensetive data like credentials. Passwords, security tokens.
[] Return proper status code according to operation you done. (e.g. 200 OK , 400 Bad Request , 401 Unauthorized, 405 Method Not Allowed ... etc)
