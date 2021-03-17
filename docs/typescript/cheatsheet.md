# TypeScript Cheat Sheet

<br>

## Types

### String

```typescript
const str: string = "hello world.";
```

<br />

### Number

```typescript
const num: number = 1;
```

<br />

### Array

```typescript
const nums: Array<number> = [1, 2, 3];
```

<br />

```typescript
const strs: Array<string> = ["1", "2", "3"];
```

<br />

```typescript
const secNums: number[] = [1, 2, 3];
```

<br />

```typescript
const tupl: [string, number] = ["1", 2];
```

<br />

### Object

```typescript
const obj: object = {};
```

<br />

```typescript
const person: object = {
	name: "Yujin",
	age: 29,
};
```

<br />

```typescript
const person: { name: string; age: number } = {
	name: "Yujin",
	age: 29,
};
```

<br />

### Boolean

```typescript
const show: boolean = true;
```

<br />

### Parameters and Return

#### Basic

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

### Any

- Type is decided on runtime with `any` type given. If the value for `noImplicitAny` property in `tsconfig.json` file, `any` type is not available unless specified explicitly.

```typescript
let val: any = 1;

val = "hello world.";

val = [1, 2];
```

<br />

### Void

- `void` means the function returns nothing.

```typescript
function logSomething(str: string): void {
	console.log(str);
}
```

<br />

---

### References
