> **Deprecation notice:** this package is deprecated in favor of [**semantic-release-vsce**](https://github.com/felipecrs/semantic-release-vsce), please use that instead.

# semantic-release-vsce

[![npm](https://img.shields.io/npm/v/@felipecrs/semantic-release-vsce.svg)](https://www.npmjs.com/package/@felipecrs/semantic-release-vsce)
[![downloads](https://img.shields.io/npm/dt/@felipecrs/semantic-release-vsce.svg)](https://www.npmjs.com/package/@felipecrs/semantic-release-vsce)
[![ci](https://github.com/felipecrs/semantic-release-vsce/workflows/ci/badge.svg)](https://github.com/felipecrs/semantic-release-vsce/actions?query=workflow%3Aci)
[![dependencies](https://david-dm.org/@felipecrs/semantic-release-vsce/status.svg)](https://david-dm.org/@felipecrs/semantic-release-vsce)
[![peerDependencies](https://david-dm.org/felipecrs/semantic-release-vsce/peer-status.svg)](https://david-dm.org/felipecrs/semantic-release-vsce?type=peer)
[![Greenkeeper](https://badges.greenkeeper.io/@felipecrs/semantic-release-vsce.svg)](https://greenkeeper.io/)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

Semantic release plugin for Visual Studio Code extensions

#### Add config to package.json

Use `@felipecrs/semantic-release-vsce` as part of `verifyConditions` and `publish`.

```json
{
  "scripts": {
    "semantic-release": "semantic-release"
  },
  "release": {
    "verifyConditions": [
      "@felipecrs/semantic-release-vsce",
      "@semantic-release/github"
    ],
    "prepare": {
      "path": "@felipecrs/semantic-release-vsce",
      "packageVsix": true
    },
    "publish": [
      "@felipecrs/semantic-release-vsce",
      {
        "path": "@semantic-release/github",
        "assets": "*.vsix"
      }
    ]
  },
  "devDependencies": {
    "semantic-release": "^17.0.0",
    "@felipecrs/semantic-release-vsce": "^2.1.0",
  }
}
```

If `packageVsix` is set, will also generate a .vsix file at the set file path after publishing. If is a string, it will be used as value for `--out` of `vsce package`.
It is recommended to upload this to your GitHub release page so your users can easily rollback to an earlier version if a version ever introduces a bad bug. 

If `yarn` is set to `true`, will use `--yarn`option for `vsce package` and `vsce publish`.

#### Working with older versions

This example is for `semantic-release` v15.  
Prior to v15, `prepare` was part of `publish` - if you are using v14, you must pass the `packageVsix` option to `publish` instead.  
Prior to v13, you had to override `getLastRelease` to use `@semantic-release/git` instead of the default `@semantic-release/npm`. This is no longer needed.

#### Travis example

Secret environment variables: `VSCE_TOKEN`

Example:

```yaml
# .travis.yml

cache:
  directories:
    - ~/.npm

script:
  - npm test

stages:
  - test
  - name: release
    if: branch = master AND type = push AND fork = false

jobs:
  include:
    - stage: release
      language: node_js
      node_js: '10.18'
      script: npm run semantic-release
```
