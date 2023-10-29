# Streams:

- callback functions which can be called
- Stream of values are emitted from click events, setInterval, setTimeout, etc
- RxJS is blue print of the stream

### Examples:

```js
document.addEventListener("click", () => {
  console.log("click event");
});

setInterval(() => {
  console.log("print me after every 1 second");
}, 1000);

setTimeout(() => {
  console.log("print me after 3 seconds");
}, 3000);
```

# How to combine these 3 streams ?

```js
document.addEventListener("click", () => {
  console.log("click event");

  setTimeout(() => {
    setInterval(() => {
      console.log("print me after every 1 second");
    }, 1000);
  }, 3000);
});
```

### Not an efficient way to combine the events, this is rxjs way:

```js
const timer$ = timer(3000, 1000);
const click$ = fromEvent(document, "click");

click$.subscribe((val) => {
  console.log("click event");
  timer$.subscribe((val) => {
    console.log("print me after every 1 second");
  });
});
```

# RXJS

### One Stream

```js
const interval$ = timer(2000, 1000); // just a definition

// we need to subscribe to use it (to stream it)
interval$.subscribe((val) => {
  console.log("sharad", val);
});
```

### Multiple Streams

```js
const interval$ = interval(1000); // just a definition

// we need to subscribe to use it (to stream it)
interval$.subscribe((val) => {
  console.log("sharad", val);
});

// we need to subscribe to use it (to stream it)
interval$.subscribe((val) => {
  console.log("sharad", val);
});
```

## interval observable

```js
const interval$ = interval(1000); // just a definition
```

## timer observable

```js
const timer$ = timer(3000, 1000); // just a definition
```

## click observable

```js
const click$ = fromEvent(document, "click"); // just a definition
```

## subscribe function

```js
const click$ = fromEvent(document, "click"); // just a definition
click$.subscribe(
  (val) => console.log(val),
  (err) => console.log(err),
  () => console.log("stream is resolved and completed")
);
```

## unsubscribe function

```js
const interval$ = interval(1000); // just a definition
const subscription = interval$.subscribe((val) => console.log(val));

subscription.unsubscribe();
```

## how to create your own observable

```js
const obervable$ = Observable.create((observer) => {
  try {
    observer.next("sharad");
    observer.complete();
  } catch (e) {
    observer.error(e);
  }

  return () => {
    console.log("executed when it is unsubscribed");
  };
});

obervable$.subscribe((val) => console.log("sharad", val));
```

# Notes:

- variable$ --> $ represents that the variable is an observable
