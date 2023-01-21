# 15 Commonly used Utility Types in Typescript

# Introduction

Utility types are built-in types in TypeScript that can manipulate and extract information from existing types. They allow you to perform operations such as picking specific properties from an object, making properties read-only, and more.

# Partial

`Partial<T>` is a utility type that makes all properties of `T` optional. This can be useful when creating a type for an object that may not have all properties set.

```typescript
interface Person {
    name: string;
    age: number;
}

function updatePerson(person: Person, updates: Partial<Person>) {
    return { ...person, ...updates };
}

const person: Person = { name: 'John Doe', age: 30 };
const updatedPerson = updatePerson(person, { age: 31 });
// updatedPerson is { name: 'John Doe', age: 31 }
```

In the example above, only the age of the person object will be updated.

# Pick

`Pick<T, K extends keyof T>` is a utility type that makes a new type by picking a set of properties from `T`. It is beneficial when creating a type for an object that is a subset of another object.

```typescript

interface Kyc {
    name: string;
    age: number;
    address: string;
}

interface School {
    name: string
    level: string
}

interface Person {
    kyc: Kyc
    school: School
}


function displayKyc(person: Pick<Person, 'kyc'> ) {
    console.log(person.school) // type error
    console.log(`Name: ${person.kyc.name}, Age: ${person.kyc.age}, Address: ${person.kyc.address}`); 
}

const person: Person = {
    kyc: {
        name: 'John Doe',
        age: 30,
        address: '123 Main St.'
    },
    school: {
        name: "Landmark",
        level: "year 2"
    }
};
displayKyc(person); // Name: John Doe, Age: 30, Address: 123 Main St.
```

The Pick type ensures you can reference the particular properties that have been cherry-picked(if you may call it). It doesn't allow you to reference the non-cherry-picked types

# Omit

`Omit<T, K extends keyof T>` is a utility type that creates a new type by omitting a set of properties from `T`. This can be useful when creating a type for an object that should not have specific properties.

```typescript
interface Person {
    name: string;
    age: number;
    address: string;
}

function displayNameAndAge(person: Omit<Person, 'address'>) {
    console.log(person.address) // error 
    console.log(`Name: ${person.name}, Age: ${person.age}`);
}

const person: Person = { name: 'John Doe', age: 30, address: '123 Main St.' };

displayNameAndAge(person); // Name: John Doe, Age: 30
```

# Exclude

`Exclude<T, U>` is a utility type that creates a new type by excluding all properties that are assignable to the type `U` from `T`. This can be useful when creating a type for an object that should not include certain properties.

```typescript
interface Person {
    name: string;
    age: number;
    address: string;
}

interface Address {
    address: string;
}

function displayNameAndAge(person: Exclude<Person, Address>) {
    console.log(`Name: ${person.name}, Age: ${person.age}`);
}

const person: Person = { name: 'John Doe', age: 30, address: '123 Main St.' };
displayNameAndAge(person); // Name: John Doe, Age: 30
```

# Extract

`Extract<T, U>` is a utility type that makes a new type by extracting all properties that are assignable to the type `U` from `T`. This can be useful when creating a type for an object that should only include specific properties.

```typescript
interface Person {
    name: string;
    age: number;
    address: string;
}

interface Address {
    address: string;
}

function displayAddress(person: Extract<Person, Address>) {
    console.log(`Address: ${person.address}`);
}

const person: Person = { name: 'John Doe', age: 30, address: '123 Main St.' };
displayAddress(person); // Address: 123 Main St.
```

# Record

`Record<K, T>` is a utility type that makes a new type with properties of type `T` and keys of type `K`. This can be useful when creating a map-like object.

```typescript
function getAge(name: string, ages: Record<string, number>) {
    console.log(`Age: ${ages[name]}`);
}

const ages: Record<string, number> = {
    'John Doe': 30,
    'Jane Doe': 25
};
getAge('John Doe', ages); // Age: 30
```

# Readonly

`Readonly<T>` is a utility type that makes all properties of `T` read-only. This can be useful when creating a type for an object that should not be modified.

```typescript
interface Person {
    name: string;
    age: number;
    address: string;
}

function displayPerson(person: Readonly<Person>) {
   console.log(`Name: ${person.name}, Age: ${person.age}, Address: ${person.address}`);
 }

const person: Readonly<Person> = { name: 'John Doe', age: 30, address: '123 Main St.' };
displayPerson(person); // Name: John Doe, Age: 30, Address: 123 Main St.
person.age = 31; // error, age is readonly
```

# NonNullable

`NonNullable<T>` is a utility type that removes null and undefined from the type `T`. This can be useful when creating a type for an object that should not have null or undefined values.

```typescript
type typeA = NonNullable<string | number | undefined>;// typeA = string | number
type typeB = NonNullable<string[] | null | undefined>; // typeB = string[]
```

# ReturnType

`ReturnType<T>` is a utility type that extracts the return type of a function `T`. This can be useful when creating a type for a function's return value.

```typescript
function getPerson(): { name: string; age: number } {
    return { name: 'John Doe', age: 30 };
}

function displayPerson(person: ReturnType<typeof getPerson>) {
    console.log(`Name: ${person.name}, Age: ${person.age}`);
}

const person = getPerson();
displayPerson(person); // Name: John Doe, Age: 30
```

# InstanceType

`InstanceType<T>` is a utility type that extracts the instance type of a constructor function `T`. This can be useful when creating a type for an object that is created by a constructor function.

```typescript
class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}

function displayPerson(person: InstanceType<typeof Person>) {
   console.log(`Name: ${person.name}, Age: ${person.age}`);
}

const person = new Person('John Doe', 30);
displayPerson(person); // Name: John Doe, Age: 30
```

# Required

`Required<T>` is a utility type that makes all properties of `T` required. This can be useful when creating a type for an object that should have all properties set.

```typescript
interface Person {
    name?: string;
    age?: number;
    address?: string;
}

function displayPerson(person: Required<Person>) {
    console.log(`Name: ${person.name}, Age: ${person.age}, Address: ${person.address}`);
}

const person: Required<Person> = { age: 30, address: '123 Main St.' }; // error property name is required

const person: Required<Person> = { name: 'tom', age: 30, address: '123 Main St.' };
displayPerson(person); // Name: John Doe, Age: 30, Address: 123 Main St.
```

# ThisType

`ThisType<T>` is a utility type that allows you to specify the `this` type of a function. This can be useful when creating a type for a function used as a method.

```typescript
interface Person {
    name: string;
    age: number;
    display: () => void;
}

const person: ThisType<Person> = {
    name: 'John Doe',
    age: 30,
    display: function() {
        console.log(`Name: ${this.name}, Age: ${this.age}`);
    }
};
person.display(); // Name: John Doe, Age: 30
```

# UnionToIntersection

`UnionToIntersection<U>` is a utility type that converts a union type `U` to an intersection type. This can be useful when creating a type for an object that should have properties from multiple types.

```typescript
interface Person {
    name: string;
}

interface Age {
    age: number;
}

function displayPerson(person: UnionToIntersection<Person & Age>) {
    console.log(`Name: ${person.name}, Age: ${person.age}`);
}

const person: UnionToIntersection<Person & Age> = { name: 'John Doe', age: 30 };
displayPerson(person); // Name: John Doe, Age: 30
```

# Intersection

`Intersection<T, U>` is a utility type that creates a new type by intersecting the properties of `T` and `U`. This can be useful when creating a type for an object that should have properties from multiple types.

```typescript
interface Person {
    name: string;
}

interface Age {
    age: number;
}

function displayPerson(person: Intersection<Person, Age>) {
    console.log(`Name: ${person.name}, Age: ${person.age}`);
}

const person: Intersection<Person, Age> = { name: 'John Doe', age: 30 };
displayPerson(person); // Name: John Doe, Age: 30
```

# Mapped Types

Mapped types in TypeScript allow you to create new types by mapping over the properties of an existing type. They are created using the `keyof` keyword and the `T[P]` type query. This can be useful when creating a type for an object that should have modified properties.

```typescript

interface Person {    
    name: string;
    age: number;
    address: string;
}

type ReadonlyPerson = { readonly [P in keyof Person]: Person[P] };

const person: Person = { name: 'John Doe', age: 30, address: '123 Main St.' };
const readonlyPerson: ReadonlyPerson = person;

console.log(readonlyPerson.name)  // 'John Doe'
readonlyPerson.name = 'Jane Doe'  // Error: Cannot assign to 'name' because it is a readonly property.
```

# Conclusion

TypeScript provides several utility types that can make working with types more efficient and less error-prone. The utility types discussed in this article include `Partial`, `Pick`, `Omit`, `Exclude`, `Extract`, `Record`, `Readonly`, `NonNullable`, `ReturnType`, `InstanceType`, `Required`, `ThisType`, `UnionToIntersection`, `Intersection`, and `Mapped Types`. Each utility type serves a specific purpose and can be used in various situations to make working with types more manageable. Understanding and utilizing these utility types can significantly improve your code's readability and maintainability. Additionally, the TypeScript documentation provides an excellent resource for understanding these utility types and many more. You can check the [official documentation](https://www.typescriptlang.org/docs/handbook/utility-types.html) for more info.