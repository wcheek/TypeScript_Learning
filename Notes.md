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
    type Animal = {
      name: string;
    };
    type Bear = Animal & {
      honey: boolean;
    };
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

- `Type Guards` provide _narrowing_. We can use `typeof`.

  - It's best with `null` to do an explicit check: `if (strs !== null)`
  - `!=` can be used to check for `null` OR `undefined`, whereas `!== null` is only for `null`
  - `"value" in x` can be used to check if the string literal `"value"` is a property of the union type `x`. Like check if a type has a method.
  - `Type predicates` are user defined type guards: `pet is Fish` is defined as a function return type, then can later be used as a type guard:

  ```ts
  function isFish(pet: Fish | Bird): pet is Fish {
    return (pet as Fish).swim !== undefined;
  }

  if (isFish(pet)) {
    pet.swim();
  } else {
    pet.fly();
  }
  ```

## More on functions

### Generic functions

- We can use generics, or type variables, to link the input type to the output type:

```ts
function firstElement<Type>(arr: Type[]): Type | undefined {
  return arr[0];
}
```

- You can also use multiple type parameters, just have to define them in the call signature using the `<>` syntax.

#### Constraining generic types

- To limit the kinds of types a type parameter can accept, we use `extends`:

```ts
function longest<Type extends { length: number }>(a: Type, b: Type) {
  if (a.length >= b.length) {
    return a;
  } else {
    return b;
  }
}
```

- I guess we are basically saying that the `Type` needs to have a property of `length` of type `number`.

## Object Types

### Optional properties

- If a property is optional, indicated by a `?` behind its name, then we need to be sure to handle the case that it's undefined. We can either check for the `undefined` case: `let xPos = opts.xPos === undefined ? 0 : opts.xPos;` or use a destructuring pattern to set default values:

```ts
function paintShape({ shape, xPos = 0, yPos = 0 }: PaintOptions);
```

### Readonly properties

- Can be specified by marking a property with `readonly`.
- Readonly properties cannot be written to during type checking.

### Index signatures

- When you don't know the names of the properties, but you do know their shape.

```ts
interface StringArray {
  [index: number]: string;
}
```

### Extending Types

- It's common to have types that are a more specific version of other types. Use the `extends` keyword to add properties:
  `interface ChildType extends ParentType {}`

```ts
interface BasicAddress {
  name?: string;
  street: string;
  city: string;
  country: string;
  postalCode: string;
}

interface AddressWithUnit extends BasicAddress {
  unit: string;
}
```

- This enables us to copy members from one type and then add new members. We can also extend from multiple types: `interface Child extends Parent1, Parent2 {}`

### Intersecting Types

- We can use `&` to intersect two types. We produce a new type that has _all_ members of both types. This is used for type alias,es whereas `extends` is used for `interfaces`

```ts
interface Colorful {
  color: string;
}
interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;
```

### Using generics with interfaces

- We can easily use a generic type variable to define an interface or a type alias with a variable type:

```ts
interface Box<Type> {
  contents: Type;
}
type Box<Type> = {
  contents: Type;
};
let appleBox: Box<Apple> = { contents: "red delicious" };
```
