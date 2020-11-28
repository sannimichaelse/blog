## Introduction to Generators

# Introduction

Imagine watching one of your favorite movies and a hear a knock at your door, **creak creak**.  Who is that? you screamed! You quickly **paused** the movie to check who is at the door and discovered it was the delivery guy who came to drop the pizza you ordered some hours ago. You went back to the movie and **played** the movie to **continue** from where you stopped. 

Did you know what you just did? You just acted as a **generator function**. 

In this article, we will be discussing generators, what are they, and how they are useful


# What are Generators

A normal function in javascript cannot be stopped before it finishes its task i.e its last line is executed. It follows something called the **run-to-completion model**.

```
function normalJavaScriptFunction() {
  console.log('I')
  console.log('cannot')
  console.log('be')
  console.log('stopped.')
}
```

The only way to exit a normal function is by returning from it or throwing an error. If you call the function again, it starts the execution from the top again

A **generator**, on the other hand, is a function that can stop midway and then continue from where it stopped i.e You can pause a generator function and make it resume when you want and it will continue from where it stopped the execution. 

```
function* generateSequence() { 
  yield 1;
  yield 2;
  yield 3;
}
```

To define a **generator** function you add the asterisk after the function keyword with the name of the function.  **yield** is an operator with which a generator can pause itself. 

Generator functions behave differently from regular ones. When such a function is called, it doesn’t run its code. Instead, it returns a special object, called “generator object”, to manage the execution.

Here, take a look:

```
function* generateSequence() { 
  yield 1;
  yield 2;
  yield 3;
}

// "generator function" creates "generator object"
let generator = generateSequence();
console.log(generator); // [object Generator]
```

The main method of a generator object is **next()**. When called, it runs the execution until the nearest yield statement.

The result of next() is always an object with two properties:

- value: the yielded value.
- done: true if the function code has finished, otherwise false.

For instance, here we create the generator and get its first yielded value:

```
function* generateSequence() { 
  yield 1;
  yield 2;
  yield 3;
}

// "generator function" creates "generator object"
let generator = generateSequence();
let one = generator.next();
console.log(one); // {value: 1, done: false}
```

Let’s call **generator.next()** again. It resumes the code execution and returns the next yield:

```javascript
let two = generator.next();
console.log(two); // {value: 2, done: false}
```
And, if we call it a third time, the execution reaches the final **yield** statement :

```javascript
let three = generator.next();
console.log(three); // {value: 3, done: true}
```

Now the generator is done. We should see it from done: true and process value:3 as the final result.

New calls to **generator.next()** don’t make sense anymore. If we do them, they return the same object: **{done: true, value: undefined}**

# Kinds of generators 
There are four kinds of generators:

1. **Generator function declarations**:
```
 function* genFunc() { ··· }
 const genObj = genFunc();
```

2. **Generator function expressions**:
```
 const genFunc = function* () { ··· };
 const genObj = genFunc();
```

3. **Generator method definitions in object literals**:
```
 const obj = {
     * generatorMethod() {
         ···
     }
 };
 const genObj = obj.generatorMethod();
```

4. **Generator method definitions in class definitions (class declarations or class expressions)**:
```
 class MyClass {
     * generatorMethod() {
         ···
     }
 }
 const myInst = new MyClass();
 const genObj = myInst.generatorMethod();
```

# Ways of iterating over a generator
As generator objects are iterable, ES6 language constructs that support iterables can be applied to them. The following three ones are especially important.

 - **for-of loop**

```
function* genFunc() {
    yield 'a';
    yield 'b';
}

for (const x of genFunc()) {
    console.log(x);
}
// Output:
// a
// b
```
- **spread operator (...)**

```
function* genFunc() {
    yield 'a';
    yield 'b';
}

const arr = [...genFunc()]; // ['a', 'b']
```

- **destructuring**

```
function* genFunc() {
    yield 'a';
    yield 'b';
}

> const [x, y] = genFunc();
> x
'a'
> y
'b'

```

# Conclusion
This is an introduction to generators and how powerful they can be. They play a lot of roles like acting as **Iterators (data producers)**, **Observers (data consumers)**, **Coroutines (data producers and consumers)**. They also have other use cases as well. In another article, I will break down the roles generators play and how we can benefit from it. I hope you learned something from this introductory article. Dont forget to like, share and drop comments. Thanks


