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

  - But `Interfaces` are preferred over type aliases.
    - The main difference is that `interfaces` are always _extendable_, meaning the can be added to later with new properties. `Type aliases` require that you make an intersection.
    - Extending an interface:
    ```ts
    interface Animal {
      name: string;
    }
    interface Bear extends Animal {
      honey: boolean;
    }
    ```
    - Extending a type with intersections:
    ```ts
    type Animal {
      name: string;
    }
    type Bear = Animal & {
      honey: boolean
    }
    ```
    - New fields can be added to an existing interface:
    ```ts
    interface Window {
      title: string;
    }
    interface Window {
      ts: TypescriptAPI;
    }
    ```

- `type assertions` allow you to tell TS more about a specific type:

```ts
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
// Also, using angle bracket syntax
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
```

- If the type assertion is too conservative, we can first assert to `any`: `const a = expr as any as T;`
- Literal types, or specifying specific values a variable can hold, can be useful by defining a union type:

```ts
let alignment: "left" | "right" | "center";
```

- Now alignment can only be assigned these three values.
- TS will assume that the properties of a declared object can change. So we need to be sure to assert the correct literal type for the compiler not to complain with EG

  > "Argument of type 'string' is not assignable to parameter of type '"GET" | "POST"'."

- `Non-null assertion`. It's important to make sure a value isn't null. We can do this with null checking, like `if (x !== null)`, or otherwise use the postfix !: `x.value!.bla()`
