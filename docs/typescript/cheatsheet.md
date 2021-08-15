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

## 2. Interface

### 2-1. Basics

```typescript
// Interface
interface User {
	name: string;
	age: number;
}

// Used like below
function postUser(user: User) {
	// ..
}

postUser({ name: "Estelle", age: 27 });
```

<br />

### 2-2. Function Interface

```typescript
// Function Interface
interface Sum {
	(a: number, b: number): number;
}

// Used like below
const sum: Sum = function (a: number, b: number): number {
	return a + b;
};
```

<br />

### 2-3. Indexing

```typescript
// Interface with typed index and value
interface StringArray {
	[index: number]: string;
}

// Used like below
const arr: StringArray = ["Estelle", "Hailey", "Dom"];
arr[0] = 10; // Error ! String Only as Pre-typed
```

<br />

### 2-4. Dictionary Pattern

```typescript
// Inteface with typed key and value
interface StringRegExDic {
	[key: string]: RegExp;
}

// Used like below
const obj: StringRegExDic = {
	cssFile: /\.css$/,
};
obj.cssFile = "css"; // Error ! RegEx Only as Pre-typed
```

<br />

## 3. Interface Extended

```typescript
// Interface
interface User {
	name: string;
	age: number;
}

// Extending User interface
interface Admin extends User {
	team: string;
}

const admin: Admin = {
	name: "estelle",
	age: 27,
	team: "Design System",
};
```

<br />

## 4. Type Aliases

### 4-1. Typing with Type Alias

```typescript
// Typing basics
const name: string = "Estelle";

// Typing with type alias
type Name = string;
const name: Name = "Estelle";
```

<br />

### 4-2. Interface vs. Type Alias

```typescript
// Interface
interface User {
	name: string;
	age: string;
}

const user: User = {
	name: "Estelle",
	age: 27,
};

// Type alias
type Person = {
	name: string;
	age: string;
};

const user: Person = {
	name: "Estelle",
	age: 27,
};
```

<br />

Type Alias is literally just naming types, not an interface, which means it is not extendable with `extends` word.

```typescript
// User is extendable since it is an interface
interface VipUser extends User {
	agreeMarketing: boolean;
}

const vipUser: VipUser = {
	name: "Estelle",
	age: 27,
	agreeMarketing: true,
};
```

<br />

## 5. Union Type (`|`)

### 5-1. Basics

Union type allows more than one type.

```typescript
function printInformation(value: string | number) {
	// Type Filtering
	if (typeof value === "string") console.log("Name : ", value);
	if (typeof value === "number") console.log("Age : ", value);
}

printInformation("Estelle");
printInformation(27);
```

<br />

### 5-2. Interface Union Type

```typescript
interface User {
	name: string;
	age: number;
}

interface VipUser extends User {
	agreeMarketing: boolean;
	email: string;
}
```

<br />

`VipUser` interface has `name` and `age` properties implicitly since it extends `User`.

```typescript
function printUser(user: User | VipUser) {
	// Can access common properties without filtering type.
	console.log("Name : ", user.name);
	console.log("Age : ", user.age);

	// Type Filtering
	if (typeof user === VipUser) {
		console.log("Agree Marketing : ", user.agreeMarketing);
		console.log("Email : ", user.email);
	}
}

printUser({ name: "Estelle", age: 27 });
printUser({
	name: "Estelle",
	age: 27,
	agreeMarketing: true,
	email: "rubyisthebestpoodleever@gmail.com",
});
```

<br />

## 6. Intersection Type (`&`)

Intersection Type must have all properties from more than one type.

```typescript
interface User {
	name: string;
	age: number;
}

interface Admin {
	office: string;
}
```

<br />

In this case, `name`, `age` and `office` properties are required.

```typescript
function printAdminUser(user: User & Admin) {
	// ..
}

printAdminUser({
	name: "Estelle",
	age: 27,
	office: "Barcelona",
});
```

<br />

## 7. Enums

### 7-1. Basics

Enum is a group of pre-defined values. The default value of an enum member is `0`, and it is `number` typed.

```typescript
enum City {
	Seoul,
	Tokyo,
}

const office = City.Seoul;
console.log(office); // 0
```

<br />

Initialize members if needed like below.

```typescript
enum City {
	Seoul = "Seoul",
	Tokyo = "Tokyo",
}

const office = City.Seoul;
console.log(office); // "seoul"
```

<br />

### 7-2. Enum as a Type

```typescript
enum City {
	Seoul = "Seoul",
	Tokyo = "Tokyo",
}

// The values defined in City enum are the only ones allowed as a parameter.
function printDepartureByCity(city: City {
	switch (city) {
		case City.Seoul: console.log("Depart at 10:00"); break;
		case City.Tokyo: console.log("Depart at 21:00"); break;
	}
})

printDepartureByCity(City.Seoul); // "Depart at 10:00"
printDepartureByCity("Barcelona"); // Error !
```

<br />

## 8. Class

### 8-1. JavaScript `class` Basics

Constructor functions are replacable with `class` syntax since ES6.

```javascript
class User {
	constructor(name, age) {
		console.log("User instance created");
		this.name = name;
		this.age = age;
	}
}

const user = new User("Estelle", 27); // "User instance created"
```

<br />

Nothing different fundamentally. Just syntax diff.

```javascript
function User(name, age) {
	console.log("User instance created");
	this.name = name;
	this.age = age;
}

const user = new User("Estelle", 27); // "User instance created"
```

<br />

### 8-2. JavaScript Prototype Basics

```javascript
const user = { name: "Estelle", age: 27 };

const admin = {};
admin.active = true;
admin.__proto__ = user;

console.log(admin); // { active: true }
console.log(admin.name); // "Estelle"
console.log(admin.age); // 27
```

<br />

### 8-3. `class` with TypeScript

- Properties must be typed and declared before constructor works.
- Properties have different accessabilities(`readonly`/`private`/`public`).

```typescript
class User {
	readonly id: string;
	private name: string;
	public nickname: string;

	constructor(name: string, nickname: string) {
		this.name = name;
		this.nickname = nickname;
	}
}
```

<br />

## 9. Generics

### 9-1. Basics

Any type is acceptable and the late-given type will work for any `T` position.

```typescript
function printValue<T>(value: T): T {
	console.log(value);
	return value;
}

function reverseString(value: string) {
	return value.split("").reverse().join("");
}

// Returning type is fixed when calling the function.
const value: string = printValue<string>("Hello World.");

const reversedValue: string = reverseString(value);
```

<br />

### 9-2. Generics vs. Union Type

What if we don't have generics syntax and just use union type to allow more than one type?

```typescript
let reversedValue: string = "";

function printValue(value: string | number) {
	console.log(value);
	return value;
}

function reverseString(value: string) {
	return value.split("").reverse().join("");
}

const value: string | number = printValue("Hello World.");

// Type check is needed
if (typeof value === "string") reversedValue = reverseString(value);
if (typeof value === "number") reversedValue = reverseString(value + "");
```

<br />

### 9-3. Generics in Interface

```typescript
interface DropdownItem<T> {
	value: T;
	selected: boolean;
}

const item: DropdownItem<string> = { value: "Estelle", selected: false };
```

<br />

### 9-4. Generics for Array

```typescript
function printTextLength<T>(arr: <T>[]): <T>[] {
	console.log(arr.length); // `length` is allowed as arr is an array type param.
	arr.forEach(item => console.log(item));
	return arr;
}

printTextLength<string>(["Estelle", "Hailey"]); // 2
```

<br />

### 9-5. Generics Extending Interface with Typed Properties

```typescript
interface Length {
	length: number;
}

function printTextLength<T extends Length>(text: T): T {
	console.log(text.length);
	return text;
}

printTextLength<string>("Estelle"); // 7
printTextLength<number>(123); // Error ! Number type has no `length` prop.
```

<br />

### 9-6. Generics Limited to Keys of Interface with `keyof` word

```typescript
interface User {
	name: string;
	age: number;
	isVip: boolean;
}

function printUserOption<T extends keyof User>(key: T): T {
	console.log(key);
	return key;
}

printUser("name");
printUser("createdAt"); // Error !
```

<br />

## 10.`tsconfig.json`

### 10-1. Basics

- `noImplicitAny` : All types must be always specified, even `any`.

- `strict`: Strict type check gets on.

- `strictFunctionTypes` : Strict type check for functions gets on, included in `strict` option if it gets `true`.

<br />

```json
{
	"compilerOptions": {
		"allowJS": false,
		"checkJS": true,
		"target": "es5",
		"lib": ["es2015", "dom", "dom.iterable"],
		"noImplicitAny": true,
		"strict": true,
		"strictFunctionTypes": true
	},
	"include": ["./src/**/*.ts"]
}
```

<br />

### 10-2. For Vue3 Projects

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

<br />

---

### References

- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
