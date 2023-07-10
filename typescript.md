TypeScript is a superset of [javascript], meaning that it adds additional features and capabilities to the language. It was developed by Microsoft and released in 2012 as an open-source project. TypeScript takes advantage of static typing, which helps compile-time error checking, code maintenance, and readability. The language is designed to bridge the gap between JavaScript and traditional object-oriented programming languages and provides strong typing, classes, interfaces, and other advanced features, making it easier to write, maintain, and scale complex applications. TypeScript code is transpiled to JavaScript, which can be executed in any environment that supports JavaScript. Overall, TypeScript is a powerful and flexible language that can greatly enhance the development process of modern web applications.

## Config
Typescript config should be located in root folder and should has name `tsconfig.json`
Most important properties:
1. `compilerOptions`: This is an object that contains a set of compiler options that specify how the TypeScript files should be compiled into JavaScript.

2. `target`: Specifies the ECMAScript target version that the TypeScript code should compile to.

3. `module`: Specifies the module code generation method used by the TypeScript compiler.

4. `lib`: Specifies the set of JavaScript libraries that TypeScript should include in the compilation process.

5. `outDir`: Specifies the output directory for the compiled JavaScript files.

6. `sourceMap`: Specifies whether the TypeScript compiler should generate source map files that enable debuggers to map between the TypeScript and JavaScript source code.

7. `declaration`: Specifies whether the TypeScript compiler should generate declaration files that describe the types and interfaces of the compiled JavaScript code.

8. `noImplicitAny`: Specifies whether the compiler should flag any implicit any types in the TypeScript code.

9. `strict`: Specifies whether the compiler should enable strict type-checking options such as `noImplicitAny`, `noImplicitThis`, and `strictNullChecks`.
		- `strictNullChecks`
		- 

10. `include`: Specifies an array of file or directory names to be included in the compilation process.

11. `exclude`: Specifies an array of file or directory names to be excluded from the compilation process.
12. `allowJs`: Allow JavaScript files to be imported inside your project, instead of just `.ts` and `.tsx` files. For example, this JS file:

Example: 
```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "sourceMap": true,
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true
  },
  "include": [
    "src/**/*"
  ]
}
```

## Narrowing

### `typeof` type guards

As we’ve seen, JavaScript supports a `typeof` operator which can give very basic information about the type of values we have at runtime. TypeScript expects this to return a certain set of strings:

- `"string"`
- `"number"`
- `"bigint"`
- `"boolean"`
- `"symbol"`
- `"undefined"`
- `"object"`
- `"function"`

### `instanceof`narrowing

JavaScript has an operator for checking whether or not a value is an “instance” of another value. More specifically, in JavaScript `x instanceof Foo` checks whether the _prototype chain_ of `x` contains `Foo.prototype`. While we won’t dive deep here, and you’ll see more of this when we get into classes, they can still be useful for most values that can be constructed with `new`. As you might have guessed, `instanceof` is also a type guard, and TypeScript narrows in branches guarded by `instanceof`s.

```
function logValue(x: Date | string) {
	if (x instanceof Date) {
		console.log(x.toUTCString());
	} else {
	      console.log(x.toUpperCase());
	}  } 
 ```

### The`never`type

When narrowing, you can reduce the options of a union to a point where you have removed all possibilities and have nothing left. In those cases, TypeScript will use a `never` type to represent a state which shouldn’t exist.

The `never` type is assignable to every type; however, no type is assignable to `never` (except `never` itself). This means you can use narrowing and rely on `never` turning up to do exhaustive checking in a `switch` statement.

For example, adding a `default` to our `getArea` function which tries to assign the shape to `never` will raise an error when every possible case has not been handled.

```
type Shape = Circle | Square;

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default:
      const _exhaustiveCheck: never = shape;
      return _exhaustiveCheck;
  }
}
```

After adding third shape we will get error, since the case is not handled
```
interface Triangle {
  kind: "triangle";
  sideLength: number;
}
 
type Shape = Circle | Square | Triangle;
 
function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default:
      const _exhaustiveCheck: never = shape;
      return _exhaustiveCheck;
  }
}
```

## Links
- https://www.typescriptlang.org/docs - docs
- https://www.typescriptlang.org/tsconfig - config