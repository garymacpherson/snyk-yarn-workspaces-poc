How to recreate issue:

1. git clone
2. `npm install -g yarn`
3. `yarn set version berry`
4. `yarn install`
5. `snyk test` // you will observe this will work without issue but with a warning that it is only testing the root project
6. `snyk test --all-projects` // fails due to no node_modules folder in the child paths (this is fine as it can be resolved by using `--yarn-workspaces`)
7. `snyk test --yarn-workspaces` // zero result

Running step 7 with the `-d` flag shows that snyk fails to pick up the workspaces correctly.

You can add/change any value you want to the `workspaces` section in the roots `package.json` and it does not change the following output. In fact you can entirely delete it and it still behaves the same.

```
 snyk-yarn-workspaces-poc git:(main) ✗ snyk test --yarn-workspaces -d
  snyk test <ref *1> { _: [ [Circular *1] ], debug: true, yarnWorkspaces: true } +0ms
  snyk-test auto detect manifest files, found 3 [
  '/Users/garymacpherson/Code/snyk-yarn-workspaces-poc/package.json',
  '/Users/garymacpherson/Code/snyk-yarn-workspaces-poc/bar/package.json',
  '/Users/garymacpherson/Code/snyk-yarn-workspaces-poc/foo/package.json'
] +0ms
  snyk-yarn-workspaces Processing potential Yarn workspaces (3) +0ms
  snyk-yarn-workspaces Processing /Users/garymacpherson/Code/snyk-yarn-workspaces-poc as a potential Yarn workspace +0ms
  snyk-yarn-workspaces /Users/garymacpherson/Code/snyk-yarn-workspaces-poc/package.json is not part of any detected workspace, skipping +0ms
  snyk-yarn-workspaces Processing /Users/garymacpherson/Code/snyk-yarn-workspaces-poc/bar as a potential Yarn workspace +0ms
  snyk-yarn-workspaces /Users/garymacpherson/Code/snyk-yarn-workspaces-poc/bar/package.json is not part of any detected workspace, skipping +1ms
  snyk-yarn-workspaces Processing /Users/garymacpherson/Code/snyk-yarn-workspaces-poc/foo as a potential Yarn workspace +0ms
  snyk-yarn-workspaces /Users/garymacpherson/Code/snyk-yarn-workspaces-poc/foo/package.json is not part of any detected workspace, skipping +0ms
  snyk-yarn-workspaces No yarn workspaces detected in any of the 3 target files. +0ms
  snyk-test Found 0 projects from 3 detected manifests +2ms



  snyk analytics {
  "args": [
    {
      "debug": true,
      "yarnWorkspaces": true
    }
  ],
  "command": "test",
  "metadata": {
    "upgradable-snyk-protect-paths": 0,
    "local": true,
    "yarnWorkspaces": {
      "scannedProjects": 0,
      "targetFiles": [
        "/Users/garymacpherson/Code/snyk-yarn-workspaces-poc/package.json",
        "/Users/garymacpherson/Code/snyk-yarn-workspaces-poc/bar/package.json",
        "/Users/garymacpherson/Code/snyk-yarn-workspaces-poc/foo/package.json"
      ],
      "packageManagers": [
        "npm",
        "npm",
        "npm"
      ],
      "ignore": [
        "node_modules"
      ]
    },
    "pluginName": "snyk-nodejs-yarn-workspaces"
  },
  "os": "macOS Monterey",
  "osPlatform": "darwin",
  "osRelease": "21.4.0",
  "osArch": "x64",
  "version": "1.834.0",
  "nodeVersion": "v14.15.4",
  "standalone": false,
  "integrationName": "",
  "integrationVersion": "",
  "integrationEnvironment": "",
  "integrationEnvironmentVersion": "",
  "id": "e382a25d26ae7132fd94203f91259e92f0bab6f0",
  "ci": false,
  "environment": {
    "npmVersion": "6.14.10"
  },
  "durationMs": 784,
  "metrics": {
    "network_time": {
      "type": "timer",
      "values": [],
      "total": 0
    },
    "cpu_time": {
      "type": "synthetic",
      "values": [
        784
      ],
      "total": 784
    }
  }
} +0ms
  snyk sending request to: https://snyk.io/api/v1/analytics/cli +0ms
  snyk request body size: 996 +0ms
  snyk gzipped request body size: 496 +0ms
  snyk not using proxy +1ms
```