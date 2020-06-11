## Observables compared to other techniques

![observables](https://rangleio.ghost.io/content/images/2016/04/observables-and-reactive-programming-in-angular-2-rangleio.gif)

## Prerequisites
To be able to follow along in this tutorial, you should have knowledge of the following
- [Understanding RxJS](https://blog.logrocket.com/understanding-rxjs-observables/)
- [Promises](https://blog.logrocket.com/improve-async-programming-with-javascript-promises-1652ac8d036d/)
- [Array Methods](https://blog.logrocket.com/understand-array-methods-by-implementing-them/)

Over the years, Javascript has evolved in the way it handles asynchronous requests ranging from callbacks, to promises and then async await. While the evolution has brought about a lot of change in the way asynchronous events are handled, it is of noteworthy to mention the power of Observables in Reactive programming and how it handles asynchronous requests dynamically and efficiently. In this article we will be comparing Observables with Promises,Arrays and Event API and why you might want to consider it in your next angular project

## Observables vs Promises

- **Observables are declarative while Promises are not**

  Obseravables allows you to define operations that will only be performed when an observer subscribes and needs the result. Promises on the other hand is executed immediately on creation. In essense, we can say _Observables are Lazy while Promises are Eager_

  **Eager Promises**

```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  title = 'observables';

  ngOnInit() {
    this.eagerPromises();
  }

  eagerPromises() {
    const greet = new Promise((resolve, reject) => {
      console.log('Inside Promise (proof of being eager)');
      resolve('Welcome! Nice to meet you.');
    });

    console.log('Before calling then on Promise');

    //greet.then((res) => console.log(`Greeting from promise: ${res}`));
  }
}
```
   Even though the promise above has not been settled, it will print the message inside the callback function. Result will be
  
```
 Inside Promise (proof of being eager
 Before calling then on Promise
```

**Lazy Observables**

```javascript
import { Component } from '@angular/core';
import { Observable } from 'rxjs';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  title = 'observables';

  ngOnInit() {
    this.lazyObservable();
  }

  lazyObservable() {
    const greet = new Observable((observer) => {
      console.log('Inside Observable (proof of being lazy)');
      observer.next('Hello! I am glad to get to know you.');
      observer.complete();
    });

    console.log('Before calling subscribe on Observable');

   // greet.subscribe({
    //   next: console.log,
    //   complete: () => console.log('End of conversation with preety lady'),
    // });
  }
}



```

In the example above the messages inside the Observable will not be fired until an Observer subscribes to the Observable. The result will be

```
Before calling subscribe on Observable
```

- **Observables differentiate between transformation function such as a map and subscription, While Promises dont**

What this means is that, Observables can clearly differentiate the kind of operation being performed on it even though the susbcription has not been called. Only subscription activates the subscriber function to start computing the values. For example, Observables will not start computing operations while a transformation is going on with the use of maps and pipes

```javascript
import { Component } from '@angular/core';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  title = 'observables';

  ngOnInit() {
    const observable = new Observable<number>((observer) => {
      observer.next(2);
      observer.complete();
    });

    observable.pipe(map((v) => 2 * v)).subscribe({
      next: console.log,
      complete: () => console.log('End of transformation'),
    });
  }
}
```
In the example above, Observables are clearly able to differentiate between transformation and subscription. The result will only become 4 when the subscribe method is called

Promises do not differentiate between the last .then clauses (equivalent to subscription) and intermediate .then clauses (equivalent to map).

```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  title = 'observables';

  ngOnInit() {
   const promise = new Promise<number>((resolve, reject) => {
      resolve(2);
    });

    promise.then((v) => 2 * v);
  }
}
```
In the Example above, the promise cannot differentiate between transformation and subscription(as in the case of Observables). It computes the value immediately even though i might only want to transform the input without computing result yet.

- **Observable subscriptions are cancellable. Unsubscribing removes the listener from receiving further values, and notifies the subscriber function to cancel work**

```javascript
import { Component } from '@angular/core';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  title = 'observables';

  ngOnInit() {
    const observable = new Observable<number>((observer) => {
      observer.next(2);
      observer.complete();
    });

    const subscription = observable.pipe(map((v) => 2 * v)).subscribe({
      next: console.log,
      complete: () => console.log('End of transformation'),
    });

    subscription.unsubscribe();
  }
}
```
Promises are not cancellable by default except its from Bluebird :smile: or you want to write a custom cancellation function

- **Observable execution errors are delivered to the subscriber's error handler, and the subscriber automatically unsubscribes from the observable**.

```javascript
import { Component } from '@angular/core';
import { Observable, of } from 'rxjs';
import { map, catchError } from 'rxjs/operators';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  title = 'observables';

  ngOnInit() {
    const observable = new Observable<any>((observer) => {
      throw Error('my error');
    });

    observable.pipe(catchError((err) => of([]))).subscribe({
      next: console.log,
    });
  }
}
```

Because of separation of concerns, angular provides an operator called catchError that allows the subscriber to handle error efficienty in the case of an error in the Observable. You can read more about it [here](https://stackoverflow.com/questions/54771154/why-handle-errors-with-catcherror-and-not-in-the-subscribe-error-callback-in-ang)

Promises push errors to the child promises.


## Observables vs Event API

- **Observables allows you to have finer control over when to start and stop listening via subscribe and unsubscribe**

**Observables**
```javascript
// Setup
let clicks$ = fromEvent(buttonEl, ‘click’);
// Begin listening
let subscription = clicks$
  .subscribe(e => console.log(‘Clicked’, e))
// Stop listening
subscription.unsubscribe();
```

**Events API**
```javascript
function handler(e) {
  console.log(‘Clicked’, e);
}
// Setup & begin listening
button.addEventListener(‘click’, handler);
// Stop listening
button.removeEventListener(‘click’, handler);
```

Using Observable allows to easily unsubscribe as opposed to removing event listener from the button 

- **Observables allows you to expose the DOM event as an observable, ie filtering or switching or combining or debouncing is easier**

**Observables**
```javascript
// Setup
let clicks$ = fromEvent(inputEl, ‘keydown’);
clicks$.pipe(map(e => e.target.value)).subscribe(e => console.log(‘Clicked’, e))
```

**Events API**
```javascript
element.addEventListener(eventName, (event) => {
  // Cannot change the passed Event into another
  // value before it gets to the handler
});
```

Observables allows transformation of inouts before there are being used while events cannot change the value until its passed to the handler

## Observables vs Arrays

**Array**
```javascript
const array: Array<number> = [1, 2, 3];
array.forEach(elt => console.log(elt));
```
This is synchronous and will execute right now


**Observable Array**
```javascript
const observable: Observable<number> = Observable.from([1, 2, 3]);
observable.subscribe(elt => console.log(elt));
```

This is asynchronous, and will execute one element at a time, as they come in.

The main difference is just a single list of values at a single point in time. An observable of arrays is a stream of arrays, each "tick" yielding a entire, new, different array.

Therefore, use an array if you just want a list of items sitting there. Of course you can mutate or transform the array, but that doesn't change the fact that at any given point in time there is just one single array.

Use an observable of arrays if you intend to continue to get new versions of the array--different versions at different points in time--and you want to "push" each new version out to different parts of your program, which in observable terminology will "subscribe" to the observable, and be notified each time it is updated.

## References
- [Promises Cancellation](https://medium.com/@benlesh/promise-cancellation-is-dead-long-live-promise-cancellation-c6601f1f5082)
- [Angular.io](https://angular.io/guide/comparing-observables)
- [RxJS Error Hanlding](https://blog.angular-university.io/rxjs-error-handling/)
