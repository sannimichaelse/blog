## Tips For Writing Maintainable Javascript

Writing clean and Maintainable Javascript might look like an elusive task for the majority. I have found myself in this situation several times and I still wonder how I will be able to fully grasp the language. The only thing I could resolve to, is to learn from Subject Experts and Understand how things work on the Subliminal level and also to learn the best practices of the Language. 

In this article, I will be explaining one of the Tips for writing Maintainable Javascript. I have a lot of other tips to share but the article can get boring, so I would probably break it into a series

# 1. Avoid Globals

The javascript execution environment is unique in a lot of ways, the environment is loaded with a lot of globals at the start of script execution all of which are available in the global context. In most browsers, the window object is overloaded to be the global object, so variables declared in the global scope becomes a property of the window object. for example


```javascript
var color = "red"

function sayColor() { 
   alert(color);
}

console.log(window.color); // red
console.log(typeof window.sayColor) // function

```

In the example above, the variable and function declaration was added to the window object properties even though there weren't set to do so 

# Why Should I Avoid Globals

#### Name Collisions

The chances for name collisions increases as the number of global variables and functions also increase. Also, The chances of using a variable already declared accidentally in the global scope also increases. The easiest code to maintain is a code in which all of the variables are declared in a local scope. The global object is also where the native Javascript objects and properties are defined and by adding your own name to the global object, you run the risk of using a name that might be later provided by the browser later on

#### Tight Coupling

A code that depends on globals is tightly coupled to the environment, if there is a change in the environment the function or variable is likely to break. The function from the previous example can be improved by passing the color as an argument to the function. All that matters is to pass a valid value to the function

# How to avoid globals
- **Use closures**
> A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). In other words, a closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time. - MDN

Given this function 
```javascript
const name = 'tomiwa';
const title = 'Software et Devops Engineer';
const company = 'Soft Signatures Lab';

function getProfileDetails(){
  return {
    name,
    title,
    company
  }
}
```

The Javascript window object has access to the const variables as well as the **getProfileDetails()** function. It made the variable and function declaration globals without our permission. Let's see

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1606080301239/vR1hNR7Nj.png)

From the console, I was able to access all the variable and function declarations globally. Now let's Encapsulate it with Closures. 

With closures the function becomes

```javascript

function profileClosure(){
  const nick_name = 'tomiwa';
  const title = 'Software et Devops Engineer';
  const company = 'Softsignatures Lab';

  return function getProfileDetails(){
     return {
       nick_name,
       title,
       company
     }
  }
}

```

You should notice that with closures only the **profileClosure** function will be accessible to the global window object. Every other thing has been encapsulated with closures. See image below

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1606080871323/xg35MjnAh.png)

- **Use Immediately invoked Function Expressions - IIFE**

Using IIFE also allows you to avoid declaring global functions and variables. With IIFE, the function above becomes 

```javascript

(function profileClosure(){
  const nick_name = 'tomiwa';
  const title = 'Software et Devops Engineer';
  const company = 'Softsignatures Lab';

  return function getProfileDetails(){
     return {
       nick_name,
       title,
       company
     }
  }
})();

```

Things get more interesting with IIFE, the **profileClosure** function is no longer available with IIFE

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1606082443254/6BibIqofI.png)

The reason why it's not seeing the **profileClosure** function is that it's seeing it as an expression. So with the brackets wrapped we are saying that, hey, this isn't a function declaration, it's a function expression. So the Javascript engine won't see function as the first item on the line, instead, it's going to see these brackets

Since the anonymous function within this IIFE is a function expression, and it's not being assigned to any global variables, no global property is really being created. And all the properties created inside of the IIFE are going to be scoped there. It’s only going to be available inside the IIFE but not outside.

But what happens if we need to access some of the functions and variables in the code?

- **Use a global object **

```javascript

var TOMIWA_PROFILE = (function () {
    const nick_name = 'tomiwa';
    const title = 'Software et Devops Engineer';
    const company = 'Softsignatures Lab';

    return {
        getProfileDetails: function() {
            return {
                nick_name,
                title,
                company
            }
        },
       getNickName: function(){
            return nick_name
       }
    }
})();

```
Using a single global object with IIFE allows you to manage your variables and functions and only return the ones needed.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1606083638535/pnmLbfgmq.png)

In the screenshot above, it's clear that we can't access the company, name, and title directly but the **getCompanyDetails** function can. This is one of the powers of closures. It makes you write code like a Ninja

### Summary
To avoid polluting the global scope
- Use closures
- Use immediately invoked function expressions - IIFE
- Use a single global object

# Conclusion
Writing maintainable javascript requires learning patterns from the expert and trying to apply what we've learned into our daily work. It can be tedious at the beginning but over time, it begins to pay off. This also shows we care about the code as well. 

If you like and learned something from this article, don't forget to like and share with your friends