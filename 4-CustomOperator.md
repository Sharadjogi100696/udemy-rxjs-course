# DebugOperator

- returns a new operator

```ts
export const debug =
  (level: string, message: string) => (source: Observable[any]) => {
    source.pipe(tap((val) => `${message} - ${val}`));
  };
```

# forkJoin Operator

- launch several tasks parallel
- wait for the completion
- get the result of each task
- wait for all the promise to get resolved

```js
forkJoin({ s1: of(1, 2), s2: of(3, 4) }).subscribe((v) => {
  console.log("one", v);
});
```

# take Operator

take the latest n outputs

```js
interval(1000).pipe(take(5));

// 0
// 1
// 2
// 3
// 4
```
