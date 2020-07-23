# Report Tests Count

- [WIP Branch](https://github.com/MLH-Fellowship/tsd/tree/feature/verbose-reporting)

## Idea

The idea is to make TSD report the number of tests it ran no matter if the tests fail or not.
The return value from TSD would be something like this. Implementing this would allow us to pass this information
to any test runners that use TSD at it's back like [AVA]() or [Jest]().

```ts
interface ExtendedDiagnostics {
    numTests: number,
    diagnostics: Diagnostic[]
}
```

Doing so, we will get an object like these:

- If tests passed

```js
{
    numTests: 1024, // Total number of tests it ran through
    diagnostics: [] // No failed test cases
}
```

- If tests failed

```js
{
    numTests: 1024, // Total number of tests it ran through
    diagnostics: [
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

If test failed, it will return an array with these fields. But they are pretty useful to understand the why tests are
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

As you can see. It does not report the number of tests it ran through. And the **only** way to know if tests passed or 
failed is by counting the length of array returned by TSD.

Because of this design choice, we cannot make TSD return `numTests` property. If we do that, developers using TSD to 
run tests programmatically have to make changes so that this breaking change is supported. Thus, we need to contact the
TSD maintainer and ask them if it's okay to introduce this breaking change.

## What can be done?

To follow along, I would suggest you to clone the
[WIP branch](https://github.com/MLH-Fellowship/tsd/tree/feature/verbose-reporting). Now go to 
[source/cli.ts](https://github.com/MLH-Fellowship/tsd/blob/544c40159e00bc5364ed941ee4ace1d402d9dec4/source/cli.ts#L28)
and you can see that the intellisense is reporting an error that it couldn't find `length` property in 
`ExtendedDiagnostics` (which is expected).

This means we need to re-write the following files (probably more) to make it support the breaking change about to be
introduced.

- Test files inside 
[source/test](https://github.com/MLH-Fellowship/tsd/tree/544c40159e00bc5364ed941ee4ace1d402d9dec4/source/test)
- The [source/cli.ts](https://github.com/MLH-Fellowship/tsd/blob/544c40159e00bc5364ed941ee4ace1d402d9dec4/source/cli.ts)
- And *mostly* every file inside
[source/lib](https://github.com/MLH-Fellowship/tsd/blob/544c40159e00bc5364ed941ee4ace1d402d9dec4/source/lib)

## How to start?

We need to get in touch with TSD's maintainer [Sam Verschueren](https://github.com/SamVerschueren). We must ask if this
type of change is okay or not. Or we can get suggesstions and then start implementing those.
