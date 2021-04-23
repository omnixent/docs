# Public REST APIs

Omnixent allows you to expose a public REST API by setting the `ENABLE_PUBLIC_API` environment variable.

## Public API limitation

The public API has several limitations:
- You can only query against Google search
- You can only query for English results
- You can only query against US results
- You can not ask for fresh results (cached by default, if cache is enabled)

## Public API request example

```bash
curl -X GET https://my-omnixent-instance.com/v1/public?term=node.js
```

Response:

```json
{
  "success": <boolean>,
  "cached": <boolean>,
  "result": {
    "term": [
      {
        "category": "term",
        "term": "node.js",
        "originalTerm": "node.js",
        "result": [
          "node.js",
          "node.js download",
          // ...
        ]
      }
    ],
    "questions": [
      {
        "category": "questions",
        "term": "why node.js",
        "originalTerm": "node.js",
        "result": [
          "why node.js is bad",
          "why node.js",
          // ...
        ]
      },
      // ...
    ]
  }
}
```

## Available parameters

| parameter | type     | default | optional |
|-----------|----------|---------|----------|
| `term`    | `string` | `null`  | `false`  |