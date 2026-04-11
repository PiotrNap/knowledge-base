### How To Empty An Array In-Place

Example:

```js
const arr = [1, 2, 3]
arr.length = 0
console.log(arr) // []
```

The above code does not create a new array object, and instead empties the original array, keeping the same reference.

On the other hand, this:

```js
let arr = [1, 2, 3]
arr = []

console.log(arr) // []
```

..though it does look like the array object has changed, what really happened is that a new array has been created and assigned to the `arr` variable.
