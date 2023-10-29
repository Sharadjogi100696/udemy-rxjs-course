# RxJS subject

- subject can be treated as observable as well as observer
- subject.asObservale() is just a derived state of subject.

```js
const sub = new Subject();

const observable$ = sub.asObservable();

observable$.subscribe(console.log);

sub.next(1);
sub.next(2);
sub.next(3);
sub.complete();
```

## Drawbacks:

- can be accidently shared with other code, which can emit the output
- do not have unsubscribe call back

## Notes:

- try not to use subjects

# BehaviorSubject

- it supports late subscriptions
- plain subject has no memory

```js
const sub = new Subject();

const observable$ = sub.asObservable();

observable$.subscribe(console.log);

sub.next(1);
sub.next(2);
sub.next(3);

setTimeout(() => {
  observable$.subscribe(console.log); // this will only get the latest value, not the one which already emitted. Since subject has no memory
}, 3000);
```

```js
// need to give initial value, because behaviour subjects are always giving values
const sub = new BehaviorSubject(0);

const observable$ = sub.asObservable();

observable$.subscribe(console.log);

sub.next(1);
sub.next(2);
sub.next(3);

// sub.complete() // if we uncomment this line, behaviour subject will not receive latest value

setTimeout(() => {
  observable$.subscribe(console.log); // receives the latest value
}, 3000);
```

# AsyncSubject

- waits until it is completed
- it also has the memory, post subscriptions will also receive the completed value

```js
const sub = new AsyncSubject();

const observable$ = sub.asObservable();

observable$.subscribe(console.log);

sub.next(1);
sub.next(2);
sub.next(3);
sub.complete();
```

# ReplaySubject

if we want to get all the value not only the latest one

```js
const sub = new ReplaySubject();

const observable$ = sub.asObservable();

observable$.subscribe(console.log);

sub.next(1);
sub.next(2);
sub.next(3);
sub.complete();
```

# first operator

only emits the first value

# withLatestFrom operator

- helps to get the latest values of all the observables passed
- should be used for any api depending on another api and needs the latest value
