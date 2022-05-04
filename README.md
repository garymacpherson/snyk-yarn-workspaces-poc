It appears to be currently not possible to run a snyk test for one child package within a root yarn workspace

How to recreate issue:

1. git clone
2. `npm install -g yarn`
3. `yarn set version berry`
4. `yarn install`
5. `snyk test` // you will observe this will work without issue but with a warning that it is only testing the root project
6. `snyk test --all-projects` // fails due to no node_modules folder in the child paths (this is expected due to the use of yarn workspaces)
7. `snyk test --yarn-workspaces` // prints the results for ALL solutions

How do you now test just `foo` or `bar`?

This is useful in the case where there are many large child packages which can take upwards of 10 minutes to finish when run on the root package.

`snyk test --file=./foo/package.json` fails because there is no node_modules
`snyk test --yarn-workspaces --file=./foo/package.json` fails because "The following option combination is not currently supported: file + yarn-workspaces"