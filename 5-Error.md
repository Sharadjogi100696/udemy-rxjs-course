# catchError

```js
// to recover the data if error occues
observable$.pipe(catchError((e) => of(1,2,3));
```

# throwError

```js
// throwError
observable$.pipe(catchError((e) => throwError(e));
```

# finalize

```js
observable$.pipe(catchError((e) => throwError(e), finalize(() => console.log()) );
```

# retryWhen

```js
// immediately tries to retry
observable$.pipe(retryWhen((err) => err));
```

```js
// immediately tries to retry after 2 sec
observable$.pipe(retryWhen((err) => err.pipe(delayWhen(() => timer(2000)))));
```

# startWith

provides initial value

```js
const a$ = interval(1000).pipe(startWith(3));
```

# throttle

in a given interval, limit the output

skips the emission till that interval, only captures the first one

```js
observable$.pipe(throttle(() => interval(500)));
```

# throttleTime

```js
observable$.pipe(throttleTime(500));
```
