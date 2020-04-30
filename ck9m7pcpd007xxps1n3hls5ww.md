## JavaScript Promises

# What is a promise?

## Imagine this…

You are the King of a medieval empire. Everything was great in your Kingdom, till your favorite messenger pigeon returns with a message and a deep wound.

![alt text](https://cdn-images-1.medium.com/max/800/1*w7HDQFGZnbifuVtgO9H3EA.png)

Your walls are strong, and your soldiers are brave, but you don't want beef with Genghis Khan. 

Your last hope is your loyal friend two kingdoms away who has an army strong enough to put fear in the heart of Genghis Khan!

You call your most trusted messenger, Artemedes, and tell him to ask your friend for aid.

Artemedes promises you that he will return with an answer and starts running towards your friend's kingdom.

Meanwhile, you cannot wait for Artemedes to come back before you make any other preparations. You have to be ready when Genghis Khan arrives.

So you give the gate protector of your kingdom the Elixir and the following instructions –

![alt text](https://cdn-images-1.medium.com/max/800/1*KdpQL_vIgRp8BbHNhoATFA.png)

You are probably wondering what happens next. But you should not be. Because as the King, you did a great job creating an async operation by sending Artemedes to your friend's Kingdom.

Artemedes gave you a promise that he will return with an answer and your gatekeeper knows what to do in a positive or negative scenario.

Congratulations King! You just created your first promise. A promise is simply the result of an asynchronous operation.

## __States of a promise__

At any time, a promise can be in one of these three states –

### __1. Pending__

A promise is pending when you start it. It hasn't been resolved or rejected. It is in a neutral state.

When you sent Artemedes (in the example above) to your friend's Kingdom, his promise to come back was pending. He just started off on his journey.

### __2. Fulfilled__

When the operation is successful, a promise is considered fulfilled or resolved.

If Artemedes were to come back with an army, he would have fulfilled his promise and resolved the issue.

### __3. Rejected__

If an operation fails, the promise is rejected.
If Artemedes came back and told you that your friend refused to help, the promise would have been rejected. Artemedes tried, but the external circumstances (your friend's response) didn't work out as planned.

## Write your first promise

![alt text](https://cdn-images-1.medium.com/max/800/1*ql-RyiNif33PQPqd5ujVHw.png)

You can do this in production code

![alt text](https://cdn-images-1.medium.com/max/800/1*fYkBFmu8wx63WzJetsL-nw.png)

or this

![alt text](https://cdn-images-1.medium.com/max/800/1*W3sjrYCWC76FmuUlDAKvpQ.png)

__NB__ : Whatever enters the then function is a resolved promise and whatever enters the catch function is a rejected promise

## Chaining Promises
A common need is to execute two or more asynchronous operations back to back, where each subsequent operation starts when the previous operation succeeds, with the result from the previous step. We accomplish this by creating a promise chain.

Here's the magic: the then function returns a new promise, different from the original
Basically chaining promises means you want to execute a serious of functions provided the previous one succeeds

Let's look at how we will do this using callback (the old way)

![alt text](https://cdn-images-1.medium.com/max/800/1*smU1vEfFVRy-2jdRXzVNYQ.png)

What is basically happening here is that function first() takes 2 as a value which will always be true, resolve it to 4, then pass it to second(4), second(4) resolves to 6, and then third(6) and resolve to 8.

The process above might be confusing to new programmers. Let's see how promises chaining came to the rescue

![alt text](https://cdn-images-1.medium.com/max/800/1*2Ufzqz6VUE1vYC1DZ2WrQA.png)

or with this which is simpler

![alt text](https://cdn-images-1.medium.com/max/800/1*xgcmjubru2yZBbRdPGMv2g.png)

Cool right???

OK unlike the example above each function execution depends on the result of the other. What if you want to execute all functions in parallel without making one depend on the other??

### Promise.all()
> The Promise.all(iterable) method returns a single Promise that resolves when all of the promises in the iterable argument have resolved or when the iterable argument contains no promises. It rejects with the reason for the first promise that rejects. - MDN JavaScript

__NB:__ promise.all() returns an array of all resolved iterables but rejects in catch function

![alt text](https://cdn-images-1.medium.com/max/800/1*UGmmSEfH4U-5s5LlkuZoUA.png)

if i reject any of the function then promises.all() will throw the result to catch.

Look at an example

![alt text](https://cdn-images-1.medium.com/max/800/1*Pwlv60a_4vBNrhGoZERgFQ.png)

### Promise.race()
>The Promise.race(iterable) method returns a promise that resolves or rejects as soon as one of the promises in the iterable resolves or rejects, with the value or reason from that promise. - MDN JavaScript

It returns immediately the first of the iterables rejects(catch block) or resolves (then block)

![alt text](https://cdn-images-1.medium.com/max/800/1*HReH9J7e0SMJdS5ca9pH9A.png)

![alt text](https://cdn-images-1.medium.com/max/800/1*b-dPUy1L1PDj5WzerBLB_g.png)

Am sure you learnt something reading this article. Thanks for your time