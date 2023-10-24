# Learning Typescript from [The Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)

- The `noImplicitAny` and `strictNullChecks` flags are the most important type checking flags.

  - `noImplicitAny` tells the type checker to error report for things that are implied `any`.
  - `strictNullChecks` checks if things are `null` or `undefined`

- Always refer to the primitive types with lowercase: `string`, `number`, `boolean`

- We can refer to an Array of a type as either eg `number[]` or using generics `Array<number>`.

- `any` is used when you want to avoid typechecking errors. TypeScript assumes you know better and disables subsequent typechecking.

- Usually it is unnecessary to explicitly annotate variables. TS will automatically _infer_ types.

- With functions, you can explicitly annotate parameter types, but it's usually not necessary to specify return types because it will infer them.

- _Optional_ properties can be specified by putting a `?` after the name: `first?: string`.
  - If you try to access an `Optional` property, it is possibly `undefined` and TS will tell you. To avoid this warning you need to check for `undefined` before using it:
  ```ts
  if (obj.first !== undefined) {
    // OK
  }
  // ALSO OKAY
  obj.first?.okayToCall();
  ```
- `Union Type`s combine two or more other types and specify the variable as being _any one_ of those union type's _members_:

  ```ts
  id: number | string;
  ```

  - Before working with a variable with a union type, we need to _narrow_ the type to avoid type errors.

  ```ts
  if (typeof id === "string") {
    // string operations
  } else if (typeof id === "number") {
    // number operations
  }
  ```

  - Other narrowing examples include using things like `Array.isArray(var)`

- `Type Aliases` are a name for any type:

```ts
type Point = {
  x: number;
  y: number;
};
```
