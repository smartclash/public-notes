# Test Types

## Work TBD

- Write docs for jest-runner-tsd
- In TSD
  - Write unit tests for relative-paths feature (PR already made)
    - [x] Update documentation
  - Make a PR for verbose reporting to the upstream **after** relative-paths PR is merged.
    - [ ] unit tests
    - [ ] update documentation
  - Get it merged
- Write tests for jest globals

## Work split

### Saurabh

- documentation for jest-runner-TSD
- Jest Typings Test (<https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/jest/jest-tests.ts>)
  - `It` global tests

#### Docs for jest-runner-tsd

- Introduction
  - a runner for jest
  - that tests types and uses tsd at the back
- How to use
  - first yarn install `yarn add jest-runner-tsd`
  - create a jest config `jest.config.js`
    - Have the runner property set to `jest-runner-tsd`

        ```js
        module.exports = {
            runner: 'jest-runner-tsd'
        };
        ```

  - Also mention about jest projects config option.
  - The type definitions file should be in the root with the name `index.d.tsd`
    - if the type def file is somewhere else, specify it's path in each test file at the top like this

      ```js
      /**
       * @type ../../custom/path/to/types.d.ts
       **/
      ```

### Karan

- Write unit tests for relative-paths branch on tsd
- Verbose-reporting branch
  - update documentation for verbose-reporting
  - write unit tests for verbose-reporting
  - make a PR

#### Unit tests for relative-paths branch

- Create `typings-custom-path` fixture that has a failing test case
  - write tests for that
  - check if the test failures were reported correctly
- Create `tests-custom-name` fixture that has a failing test case
  - write tests for that
  - check if the test failures are reported correctly

If possible, also work on adding checks that makes sure that the files
mentioned in the options exist or not and then throw an error if it doesn't.
