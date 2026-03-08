# TypeScript Frontend

Arth supports TypeScript as a first-class frontend language. Write `.ts` files that compile through the same pipeline as Arth — sharing types, the same IR, and the same backends.

## How It Works

```
TypeScript (.ts) ──→ arth-ts-frontend ──→ AST ──→ HIR
                                                    ↓
Arth (.arth) ──────→ arth parser ──────→ AST ──→ HIR
                                                    ↓
                                          Type Checking
                                          Ownership Analysis
                                                    ↓
                                              IR (SSA)
                                                    ↓
                                        VM / Cranelift / LLVM
```

TypeScript files are parsed by `arth-ts-frontend`, lowered to the same HIR as Arth code, and then proceed through the shared compilation pipeline — name resolution, type checking, IR lowering, and code generation.

## Supported TypeScript Subset

Arth compiles a practical subset of TypeScript:

### Variables and Types

```typescript
let name: string = "Alice";
const age: number = 30;
let active: boolean = true;

// Type inference
let count = 42;
```

### Functions

```typescript
function greet(name: string): string {
    return "Hello, " + name;
}

function add(a: number, b: number): number {
    return a + b;
}
```

### Classes

TypeScript classes map to Arth structs + modules:

```typescript
class User {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    greet(): string {
        return "Hi, I'm " + this.name;
    }
}

const user = new User("Alice", 30);
console.log(user.greet());
```

### Arrays and Objects

```typescript
let nums: number[] = [1, 2, 3];
let first = nums[0];
let len = nums.length;

let config = { host: "localhost", port: 8080 };
console.log(config.host);
```

### Control Flow

```typescript
if (age >= 18) {
    console.log("Adult");
} else {
    console.log("Minor");
}

for (let i = 0; i < 10; i++) {
    console.log(i);
}

while (active) {
    // ...
}
```

### JSON

```typescript
let data = JSON.parse('{"name": "Alice"}');
let json = JSON.stringify({ age: 30 });
```

## Mixed Codebases

You can have both `.arth` and `.ts` files in the same project. They compile through the same pipeline and can reference shared types:

```
my-project/
├── arth.toml
├── src/
│   ├── core/
│   │   └── Models.arth        # Data models in Arth
│   ├── services/
│   │   └── UserService.arth   # Backend logic in Arth
│   └── controllers/
│       └── api.ts             # API controller in TypeScript
```

## Why TypeScript?

TypeScript gives teams a familiar on-ramp:

- **Web developers** can contribute backend logic using syntax they already know
- **Gradual adoption** — start with TS, migrate performance-critical paths to Arth
- **Shared compilation** — one toolchain, one type system, one binary output
- **Same safety guarantees** — TS code goes through the same ownership and type analysis as Arth

## Running TypeScript

```bash
# Run a TypeScript file through Arth
arth run controller.ts

# Build to bytecode
arth build controller.ts

# Build to native binary
arth build --backend llvm controller.ts
```

## Current Coverage

The TypeScript frontend is actively evolving. Currently supported:

- Variable declarations (`let`, `const`)
- Functions and classes
- Basic types (`string`, `number`, `boolean`)
- Arrays and object literals
- Control flow (`if`/`else`, `for`, `while`)
- `console.log` and `JSON.parse`/`JSON.stringify`
- String concatenation and template basics

Features being added:

- Interfaces and type aliases
- Async/await
- Destructuring
- Generics
- Module imports/exports
