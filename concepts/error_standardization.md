## Error Standardization in APIs

**Summary**:
Most "serious" APIs converge to the same error structure, because it optimizes for:
debuggability, automation, and DX.

They divide them into three layers:

1. HTTP Layer - it answers the question of "What class of problem is this?".
   Eg. 400, 401, 404, 5xx, ... . This is for infrastructure + generic tooling.
2. Machine Readable Code - this answers "What _exactly_ failed?". It is represented as
   a stable string, eg: "invalid_user_input", "rate_limit_exceeded", "insufficient_fund".
3. Human message + Metadata - this answers "Why did it failed and how to fix it?". This is
   for developers and logs.

**Example Structure**:

```json
{
  "error": {
    "code": "email_invalid_input",
    "status": 422,
    "message": "Unable to process the user input",
    "category": "validation",
    "fields": [{ "field": "email", "code": "invalid_format" }],
    "request_id": "...",
    "retryable": false
  }
}
```

This structure wons because it satisfies all stakeholders:

1. Clients (no parsing strings)

```js
if (err.code === "rate_limit_exceeded") retry()
if (err.code === "invalid_api_key") logout()
```

2. Operations / Support  
   `request_id → logs → trace → DB → root cause`
   That's why request_id is everywhere

This gives the ability to change message, translate, improve wording, without breaking the client.
