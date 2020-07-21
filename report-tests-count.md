# Report Tests Count

- [WIP Branch](https://github.com/MLH-Fellowship/tsd/tree/feature/verbose-reporting)

## Idea

The idea is to make TSD report the number of tests it ran no matter if the tests fail or not.
The return value from TSD would be something like this. Implementing this would allow us to pass this information
to any test runners that use TSD at it's back like [AVA]() or [Jest]().

```ts
interface ExtendedDiagnostics {
    numTests: number,
    diagnosis: Diagnostic[]
}
```

Doing so, we will get an object like these:

- If tests passed

```js
{
    numTests: 1024, // Total number of tests it ran through
    diagnosis: [] // No failed test cases
}
```

- If tests failed

```js
{
    numTests: 1024, // Total number of tests it ran through
    diagnosis: [
        {
            fileName: 'add.test-d.ts',
            message: 'A message here',
            severity: 'error',
            line: 10,
            column: 34,
        },
        ...
    ]
}
```

## Problem

TSD library just runs the test. It does not report the number of tests it ran through. It does not report the number 
of skipped tests. Nothing. All it does is return a null array if there is no failed tests.

If test failed, it will return an array with these fields. But they are pretty useful to understand the why test are
failing.

```ts
interface Diagnostic {
  fileName: string;
  message: string;
  severity: "error" | "warning";
  line?: number;
  column?: number;
}
```

---

You call TSD, it finds all the test files and runs them. It's can be used like this to programmatically run through the 
tests.

```js
const diagnosis = await tsd();

// If length > 0, it means tests failed. Has an array of Diagnostic object.
if (diagnosis.length) {
    console.log('There are failed tests.', diagnosis);
    return ;
}

console.log('All tests passed. Yay!');
```

As you can see. It does not report the number of tests it ran through. And the **only** way we know that tests are 
passed is by counting the length.

Because of this design choice, we cannot make TSD return `numTests` property. If we do that, developers using TSD to 
run tests programmatically have to make changes so that this breaking change is supported. Thus, we need to contact the
TSD maintainer and ask them if it's okay to introduce this breaking change.

