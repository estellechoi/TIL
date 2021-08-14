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

const user: User = fetchUser(id);
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

Type Alias is literally just naming types, not creating types and not extendable with `extends` word unlike interface.

```typescript
// User is extendable as it is an interface, a customized type
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
function updateValue(value: string | number) {
	// Type Filtering
	if (typeof value === "string") console.log("name updated : ", value);
	if (typeof value === "number") console.log("age updated : ", value);
}

updateValue("Estelle");
updateValue(27);
```

<br />

### 5-2. Interface

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

```typescript
function updateUser(user: User | VipUser) {
	// Common properties can get a value without filtering type.
	console.log("Name : ", user.name);
	console.log("Age : ", user.age);

	// Type Filtering
	if (typeof user === VipUser) {
		console.log("Agree Marketing : ", user.agreeMarketing);
		console.log("Email : ", user.email);
	}
}

updateUser({ name: "Estelle", age: 27 });
updateUser({
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

```typescript
function updateUser(user: User & Admin) {
	// Common properties can get a value without filtering type.
}

updateUser({
	name: "Estelle",
	age: 27,
	office: "Barcelona",
});
```

<br />

## 7. Enums

### 7-1. Basics

Enum is a group of pre-defined values.

```typescript
enum City {
	Seoul,
	Tokyo,
}

const office = City.Seoul;
console.log(office); // 0
```

<br />

Enum members get number type `0` value if not initialized with specific value.

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

function fetchFlightByCity(city: City {
	switch (city) {
		case City.Seoul: console.log("Depart at 10:00"); break;
		case City.Tokyo: console.log("Depart at 21:00"); break;
	}
})

fetchFlightByCity(City.Seoul); // "Depart at 10:00"
fetchFlightByCity("Barcelona"); // Error !
```

<br />

## 8. Class

### 8-1. JavaScript `class` Basics

Constructor functions are replacable with `class` syntax.

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

Nothing different fundamentally. Just syntax.

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

- Properties must be typed before constructor.
- Properties have different accessabilities.

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

Any type is acceptable and the late-given type will work for any `T` position.

```typescript
function printUser<T>(user: T): T {
	console.log(user);
	return user;
}

printUser<User>(user);
printUser<Admin>(admin);
```

<br />

## 10.`tsconfig.json`

### 10-1. Basics

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

### 10-2. For Vue3

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
