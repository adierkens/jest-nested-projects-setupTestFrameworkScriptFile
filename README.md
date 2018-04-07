https://github.com/facebook/jest/issues/5938

This project demonstrates a bug in jest when using multiple packages and a setupTestFrameworkScriptFile file.

There's 2 tests under `packages/` that each require `toBeEmpty()` from `jest-extended`

The `setupTestFrameworkScriptFile` in the root `package.json` points to `jest-extended`

## To Repro

```bash
> yarn install
> yarn test
```

You should get 2 failing tests:

```
 % yarn test
yarn run v1.5.1
$ jest
 FAIL  packages/sub-package/__tests__/subpackage.test.js
  ● foo

    TypeError: expect(...).toBeEmpty is not a function

      1 | test('foo', () => {
    > 2 |     expect('').toBeEmpty();
      3 | });
      4 |

      at Object.<anonymous>.test (__tests__/subpackage.test.js:2:16)

 FAIL  packages/sub-package-2/__tests__/subpackage.test.js
  ● foo

    TypeError: expect(...).toBeEmpty is not a function

      1 | test('foo', () => {
    > 2 |     expect('').toBeEmpty();
      3 | });
      4 |

      at Object.<anonymous>.test (__tests__/subpackage.test.js:2:16)

Test Suites: 2 failed, 2 total
Tests:       2 failed, 2 total
Snapshots:   0 total
Time:        1.153s
Ran all test suites in 2 projects.
error An unexpected error occurred: "Command failed.
Exit code: 1
Command: sh
Arguments: -c jest
Directory: /Users/adierkens/Developer/jest-nested-projects-setupTestFrameworkScriptFile
Output:
".
info If you think this is a bug, please open a bug report with the information provided in "/Users/adierkens/Developer/jest-nested-projects-setupTestFrameworkScriptFile/yarn-error.log".
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```


If you remove one of the sub-projects:

```
rm -rf packages/sub-package-2/
```

and re-run the tests, the remaining package passes

```
 % yarn test
yarn run v1.5.1
$ jest
 PASS  packages/sub-package/__tests__/subpackage.test.js
  ✓ foo (4ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.815s, estimated 1s
Ran all test suites.
✨  Done in 1.24s.
```
