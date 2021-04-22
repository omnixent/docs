# Authorization

At the moment, omnixent only accepts JWT as an authorization method.
**JWT must be passed with a custom `x-omnixent-auth` HTTP header.**

Just like with all the other configurations, you can configure JWT authorization with an environment variable:

```
OMNIXENT_JWT_SECRET={"type":"HS256","key":"cKKgafNNj7vPsA4tDyqB8r9WXpEZPfru"}
```

It must be a valid JSON containing the following properties:

- `type`: the algorithm used for encrypting your JWT. Can be one of: `HS256`,`HS384`, `HS512`, `RS256`, `RS384`, `RS512`, `PS256`, `PS384`, `PS512`, `ES256`,`ES384`, `ES512`.
- `key`: the encryption key

# Example usage

```javascript
fetch('https://my-omnixent-instance.com/v1/private?service=google&term=rolex&country=us&language=en', {
  method: 'GET',
  headers: {
    'x-omnixent-auth': myJWT
  }
})
```