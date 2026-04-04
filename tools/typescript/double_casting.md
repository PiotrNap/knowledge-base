## Double Casting a Type

**Summary**:
Double casting happens when you force an unrelated type on a value by going through an `unknown` or `any` type in between.

**Explanation**:
In Typescript you can't assign unrelated types like such:

```ts
const num = 50 as string // <--- this won't work
```

.. so people double cast:

```ts
const num = 50 as unknown as string // <--- this works
```

Why? Because `unknown` is the "safe top-level" type. Everything can become `unknown`, or come from `unknown`.

**Use Cases**:

1. When dealing with badly typed libs:

```ts
const data = JSON.parse(str) as unknown as MyType
```

Typescript can't infer JSON type at runtime, so you enforce it.

2. When working with DOM elements:

```ts
const el = document.getElementById("x") as unknown as HTMLInputElement
```

Typescript can't know exactly which element will get selected ahead of time.

3. When migrating from legacy code (like JS to TS)

```ts
const oldConfig = newConfig as unknown as newConfig
```

**When (Not) To Use**
Use it only when you have 100% control over the source code, you validated the type earlier,
or there's no better way to assign the type.

Never use it as default, instead validate the type during runtime:

```ts
function isUser(x: any): x is User {
  return typeof x.id === "string"
}

app.post("/user", (req, res) => {
  if (!isUser(req.body)) {
    return res.status(400).send("invalid")
  }

  saveUser(req.body)
})
```

.. or use zod to validate input fields:

```ts
const user = UserSchema.parse(data)
```
