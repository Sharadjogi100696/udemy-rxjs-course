# Operators

## pipe operator

allows to chain multiple operators to produce new observable

```js
observable$.pipe();
```

## map operator

transforms the data

```js
observable$.pipe(map((x) => x * 10));
```

## filter operator

filter the data

```js
observable$.pipe(filter((x) => x == "1"));
```

## tap operator

produces side effects in the chain/ change any outside value/ logging

```js
observable$.pipe(tap(() => console.log(...)));
```

## shareReplay operator

### Problem:

```js
const observable$ = http();

observable$.subscribe();
observable$.subscribe();
observable$.subscribe();

// new instance (problem when dealing with api calls, 3 calls in the network for the same data)
```

```js
const observable$ = http();
const sharedObservable$ = observable$.pipe(shareReplay());

sharedObservable$.subscribe();
sharedObservable$.subscribe();
sharedObservable$.subscribe();
```

```js
const observable$ = interval(1000);
const sharedObservable$ = observable$.pipe(shareReplay());
const click$ = fromEvent(document, "click");

sharedObservable$.subscribe((x) => console.log("one", x));

click$.subscribe((val) => {
  console.log("click event");
  sharedObservable$.subscribe((val) => {
    console.log("two", val);
  });
});
```

## concat operator

```js
const a$ = of(1, 2, 3);
const b$ = of(4, 5, 6);
const c$ = of(7, 8, 9);

// serial
const d$ = concat(a$, b$, c$);
```

## merge operator

Flattens multiple observables into 1

```js
// parallel
const timer1$ = interval(3000);
const timer2$ = interval(1000);
const merged$ = merge(timer1$, timer2$);
```

## concatMap operator (dependency on other)

When we create new observable from the values of an observable and concat in a order

Generally should be used to save the form data. When we click on save 3 times, and if we use mergeMap, three http calls will be requested but with the help of concat map, we are waiting for the first request to complete.
If we use simple map, it wont wait for the transformation.


```js
fromEvent(document, "click").pipe(concatMap((x) => of(1, 2)));
```



## mergeMap operator (dependency on other)

When we create new observable from the values of an observable and concat in parallel

```js
const b$ = timer(3000, 1000).pipe(
  mergeMap((x) => {
    console.log("completed");
    return of(4, 5).pipe(tap(console.log));
  })
);
```

## exhaustMap operator

### Problem Statement

when concatMap is used to save data on click and if user clicks the button 10 times, 10 request will subscribe

exhaustMap ignores any subscription if the first subscription is not completed

```js
const click$ = fromEvent(document, "click");
const b$ = click$.pipe(
  exhaustMap((x) => {
    return interval(1000).pipe(take(5));
  })
);
```

if we replace exhaustMap with concatMap, on each click, an event will be queued

## debounce operator and distictUntilChanged

waits for a stable value and then emits the event

generally used for search events

```js
const click$ = fromEvent(formElem, "keyup").pipe(
  map((e) => e.target.value),
  debounceTime(400), // 400ms
  distinctUntilChanged() // no duplicates
);
```

## switchMap operator

cancel the ongoing req and subscribe new

```js
const click$ = fromEvent(document, "click");
const b$ = click$.pipe(
  switchMap((x) => {
    return interval(1000);
  })
);
```
