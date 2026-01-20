## Async Handler in Express.js

**Summary**:
Express does not catch rejected promises automatically. Therefore, each uncatched error
inside the app will cause the program to crash, or requests to hang.

**Solutions**:

- The ugly/naive way:

```js
app.get("/user", async (req, res, next) => {
  try {
    const users = await getUsers()
    return res.json(users)
  } catch (e) {
    next(e)
  }
})
```

The problem with this way of handling it is that it's repetitive and people often forget to
implement it.

- The right way:

```ts
import { RequestHandler } from "express"

const asyncHandler = async (fn: RequestHandler): RequestHandler => {
  return (req, res, next) => Promise.resolve(fn(req, res, next)).catch(next)
}

app.get("/user", asyncHandler(async (req, res, next) => {
    const users = await getUsers()
    return res.json(users)
})

// Error middleware is required. Async handler is useless w/out it.
app.use((err, req, res, _next) => {
    console.error(err)
    res.status(500).json({error: err.message})
})
```

Now every business logic error will go to the error middleware, without leaking information
about the stack trace.

**Common Mistakes**:

- mixing asyncHandler with try/catch randomly
- wrapping non-async handlers
- catching errors inside controllers and not rethrowing
- returning res.json() **and** calling next()
