# TypeScript Cheat Sheet

<br>

## 1. Types

### 1-1. Basics

```typescript
// String
const str: string = "hello world.";

// Number
const num: number = 1;

// Array
const nums: Array<number> = [1, 2, 3];
const strs: Array<string> = ["1", "2", "3"];
const secNums: number[] = [1, 2, 3];
const tupl: [string, number] = ["1", 2];

// Object
const obj: object = {};
const person: object = {
	name: "Yujin",
	age: 29,
};
const person: { name: string; age: number } = {
	name: "Yujin",
	age: 29,
};

// Boolean
const show: boolean = true;
```

<br />

### 1-2. Parameters and Return Type

#### Typed Parameters

- Not only type but the number of parameters is limited automatically.

```typescript
function sum(a: number, b: number): number {
	return a + b;
}
```

<br />

#### Type Inference

- Type inferenced with typed parameters

```typescript
function sum(a: number, b: number) {
	return a + b;
}
```

<br />

#### Optional Parameters

- Parameters optional

```typescript
function log(a: string, b?: string, c?: string) {
	// ...
}

log("hello");

log("hello", "world");

log("hello", "world", "!");
```

<br />

#### Typed Return

```typescript
function print(): string {
	return "hello world.";
}
```

<br />

### 1-3. Any

- Type is decided on runtime with `any` type given. If the value for `noImplicitAny` property in `tsconfig.json` file, `any` type is not available unless specified explicitly.

```typescript
let val: any = 1;

val = "hello world.";

val = [1, 2];
```

<br />

### 1-4. Void

- `void` means the function returns nothing.

```typescript
function logSomething(str: string): void {
	console.log(str);
}
```

<br />

## 2.`tsconfig.json`

### 2-1. Basics

```json
{
	"compilerOptions": {
		"allowJS": false,
		"checkJS": true,
		"noImplicitAny": true // any 타입이라도 명시
	},
	"include": ["./src/**/*.ts"]
}
```

<br />

### 2-2. For Vue3

```json
{
	"compilerOptions": {
		"target": "esnext",
		"module": "esnext",
		"strict": true,
		"jsx": "preserve",
		"importHelpers": true,
		"moduleResolution": "node",
		"skipLibCheck": true,
		"esModuleInterop": true,
		"allowSyntheticDefaultImports": true,
		"noImplicitAny": true,
		"sourceMap": true,
		"baseUrl": ".",
		"types": ["webpack-env"],
		"paths": {
			"@/*": ["src/*"]
		},
		"lib": ["esnext", "dom", "dom.iterable", "scripthost"]
	},
	"include": [
		"src/**/*.ts",
		"src/**/*.tsx",
		"src/**/*.vue",
		"src/**/*.d.ts",
		"tests/**/*.ts",
		"tests/**/*.tsx"
	],
	"exclude": ["node_modules"]
}
```

<br />

---

### References

- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
