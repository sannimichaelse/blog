## Asynchronous King

I once wrote a [story](https://sannimichaelse.github.io/blog/2018/06/20/javascript-promises/) on the power of promises and async functions in Javascript and how it can allow you to perform asynchronous operations in your code like a Pro. Recently, I had to explain the concepts of promises and async/await to some students I was training and I was looking for a way to break it down into bits for them. I decided to check out the post and use the story, but this time I converted the story into a code.

Let's go over the story again.

**Imagine this…**

You are the King of a medieval empire. Everything was great in your Kingdom, till your favorite messenger pigeon returns with a message and a deep wound.

> Give us your elixir of power or we will destroy your kingdom - Genghis Khan

Your walls are strong, and your soldiers are brave, but you don’t want beef with Genghis Khan.

Your last hope is your loyal friend two kingdoms away who has an army strong enough to put fear in the heart of Genghis Khan!

You call your most trusted messenger, Artemedes, and tell him to ask your friend for aid.

Artemedes promises you that he will return with an answer and starts running towards your friend’s kingdom.

Meanwhile, you cannot wait for Artemedes to come back before you make any other preparations. You have to be ready when Genghis Khan arrives.

So you give the gate protector of your kingdom and the Elixir the following instructions –

> If Artemedes Arrives with an army of positive response from my loyal friend, seal the gates and prepare for battle but,
> if Artemedes comes back with a negative message, give the Elixir to Genghis Khan

You are probably wondering what happens next. But you should not be. Because as the King, you did a great job creating a async operation by sending Artemedes to your friend’s Kingdom.

Artemedes gave you a promise that he will return with an answer and your gatekeeper knows what to do in a positive or negative scenario.

Congratulations King! You just created your first promise.

```javascript
async function artemedesPromise() {
  return new Promise((resolve, reject) => {
    const didYouGetResponseFromMyFriend = false;
    if (didYouGetResponseFromMyFriend) {
      resolve("Yes");
    } else {
      reject("No");
    }
  });
}
```

```javascript
async function artemedesPromise()
```

In Javascript when a function has an async signature it means the function will return a Promise. A promise is the result of an async operation. A promise can be resolved, rejected, or be in a pending state. When it's rejected it means the promise was not fulfilled and when it resolves it means the promise was fulfilled.

**We will be focusing on reject and resolve the state of a promise in this tutorial**

In this case **artemedesPromise()** is an async function. From the story, you can remember that artemedes promises to return with a result which can be a yes or no from your friend. When its a yes it means your friend has decided to help you in battle and when its a no, it means your friend will not help you

```javascript
const didYouGetResponseFromMyFriend = false;
```

In this line, we are assuming your friend decides not to help you so it means that artemedes will come back with a negative result. In this case the promise will not be fulfilled

```javascript
async function gateKeeperPromise() {
  try {
    await artemedesPromise();
    return "Seal the gates and prepare for battle";
  } catch (negative) {
    return "Give Elixir to Genghis Khan";
  }
}
```

```javascript
gateKeeperPromise();
```

As a wise king, you didn't want to put all your eggs in one basket and told your gatekeeper to perform an operation depending on the result of the asynchronous operation(Promise) being carried out by Artemedes

So it means that the function of your gatekeeper is also a Promise because his action depends on the response from Artemedes which is to lock the gates and prepare for battle when he gets positive feedback or give the elixir to Genghis Khan in case of a negative feedback

```javascript
await artemedesPromise();
```

In this line, the gatekeeper is waiting for the result of **artemedesPromise** to carry out his own action. Notice the try/catch block, it's basically used to handle the result of an async operation. When you are waiting for a promise and the promise was fulfilled, it will enter into the try block but when the promise is not fulfilled it will go into the catch block.

So in this case, if Artemedes gets positive feedback from your friend then your gatekeeper will Seal the gates and prepare for battle but if it's a negative feedback, the gatekeeper will Give your Elixir of Power to Genghis Khan

```javascript
gateKeeperPromise()
  .then((positive) => {
    console.log(positive);
  })
  .catch((negative) => {
    console.log(negative); //"Give the Elixir to Genghis Khan"
  });
```

In the line above gatekeeper carries out his own action based on the result and feedback he gets from Artemedes.

**The action carried out by the gatekeeper will be to give the Elixir to Genghis Khan because he got negative feedback from Genghis khan**

This is a simple introduction to promises and async/await. You can learn more from here

- [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Async/Await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_functions)

I hope you learned something.

## FULL CODE

```javascript
async function artemedesPromise() {
  return new Promise((resolve, reject) => {
    const didYouGetResponseFromMyFriend = false;
    if (didYouGetResponseFromMyFriend) {
      resolve("Yes");
    } else {
      reject("No");
    }
  });
}

async function gateKeeperPromise() {
  try {
    await artemedesPromise();
    return "Seal the gates and prepare for battle";
  } catch (negative) {
    return "Give the Elixir to Genghis Khan";
  }
}

gateKeeperPromise()
  .then((positive) => {
    console.log(positive);
  })
  .catch((negative) => {
    console.log(negative); //"Give the Elixir to Genghis Khan"
  });
```

**Inspired by [Enyata](https://enyata.com/) Academy 3.0**
