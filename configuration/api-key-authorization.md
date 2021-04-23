# API Key Authorization

**API Keys must be passed with a custom `x-omnixent-auth` HTTP header.**

You can configure API Key authorization using an environment variable:

```
OMNIXENT_API_KEY=["MXwGgX1qR9Nrn3gFAvMGg6EYZfLtJC","JHgjQporKoi9rCD1wqkNNAirVBzRod","nK0UtTBc0UJxXaKGF9VvmxJBCyyLrp"]
```

The `OMNIXENT_API_KEY` environment variable **must** contain a valid JSON representing an array of strings, where each string is a valid API key.

# Example usage

```javascript
fetch('https://my-omnixent-instance.com/v1/private?service=google&term=rolex&country=us&language=en', {
  method: 'GET',
  headers: {
    'x-omnixent-auth': "MXwGgX1qR9Nrn3gFAvMGg6EYZfLtJC"
  }
})
```