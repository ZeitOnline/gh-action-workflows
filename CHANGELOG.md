# Changelog

## [2.5.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.4.2...2.5.0) (2025-08-29)


### Features

* **commit-lint:** add inputs to commit-lint workflow ([#48](https://github.com/ZeitOnline/gh-action-workflows/issues/48)) ([db20672](https://github.com/ZeitOnline/gh-action-workflows/commit/db20672a26f1c809d1710d4b98632d2c48385d62))
* **commit-lint:** allow passing config etc. to the commit-lint workflow ([c3bcc14](https://github.com/ZeitOnline/gh-action-workflows/commit/c3bcc1453d721ec6ebe73ff456c3ea1316b40f49))

## [2.4.2](https://github.com/ZeitOnline/gh-action-workflows/compare/2.4.1...2.4.2) (2025-08-21)


### Miscellaneous Chores

* upgrade package dependencies ([7ed868a](https://github.com/ZeitOnline/gh-action-workflows/commit/7ed868a11bcdc08cfd63a926e62ebf1ae33e53ef))

## [2.4.1](https://github.com/ZeitOnline/gh-action-workflows/compare/2.4.0...2.4.1) (2025-07-17)


### Bug Fixes

* **nighwatch:** scan security only on PRs not main ([cc11024](https://github.com/ZeitOnline/gh-action-workflows/commit/cc11024ef6960f4653fb4c2faca65d6631f0afe0))

## [2.4.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.3.0...2.4.0) (2025-07-17)


### Features

* **nightwatch:** scan for security issues [BEM-216] ([a65b938](https://github.com/ZeitOnline/gh-action-workflows/commit/a65b938f2336c0c3308f26f9612c01ce52d9c610))
* **nightwatch:** support pypi-zon mirror [BEM-210] ([b624be2](https://github.com/ZeitOnline/gh-action-workflows/commit/b624be254d04eb0aa1e64ca2b0f30690e6bf5934))

## [2.3.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.2.1...2.3.0) (2025-05-23)


### Features

* **nightwatch:** allow to set the environment ([#34](https://github.com/ZeitOnline/gh-action-workflows/issues/34)) ([7d0c359](https://github.com/ZeitOnline/gh-action-workflows/commit/7d0c3595b7c2932f71e22aa4936902f6f59164a9))

## [2.2.1](https://github.com/ZeitOnline/gh-action-workflows/compare/2.2.0...2.2.1) (2025-04-10)


### Bug Fixes

* add issues write permission to release-please job ([#28](https://github.com/ZeitOnline/gh-action-workflows/issues/28)) ([7201356](https://github.com/ZeitOnline/gh-action-workflows/commit/720135651890608dfe97a45b7b640343b94ce6e0))

## [2.2.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.1.0...2.2.0) (2025-04-10)


### Features

* **nightwatch:** assume we do want the change if it is pushed on main ([#29](https://github.com/ZeitOnline/gh-action-workflows/issues/29)) ([d94da43](https://github.com/ZeitOnline/gh-action-workflows/commit/d94da43619ad99160e5307ba0d041aa15f504155))

## [2.1.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.0.0...2.1.0) (2025-03-21)


### Features

* add workflow to lint commit messages with 'commitlint' ([5f91b76](https://github.com/ZeitOnline/gh-action-workflows/commit/5f91b761c85c28cbbc7eb896285031b5addf7463))
* **nightwatch:** always mount the nighwatch secret, keeping it optional ([#24](https://github.com/ZeitOnline/gh-action-workflows/issues/24)) ([4b139e1](https://github.com/ZeitOnline/gh-action-workflows/commit/4b139e181d1bde19d7ea236b1bc1563ef80d6838))

## [2.0.0](https://github.com/ZeitOnline/gh-action-workflows/compare/v1.18.0...2.0.0) (2024-12-19)


### ⚠ BREAKING CHANGES

* remove no longer needed workflows

### Features

* **release:** integrate "release-please" action into build workflow ([48ad7fd](https://github.com/ZeitOnline/gh-action-workflows/commit/48ad7fd9e7e6409461e8dbd7b970292ac5e858e1))
* **release:** set up "release-please" for future releases ([b55aadd](https://github.com/ZeitOnline/gh-action-workflows/commit/b55aaddc73cb2534e14718388922e091869d3ead))
* **release:** use the conventional name for change log files by default ([1526248](https://github.com/ZeitOnline/gh-action-workflows/commit/1526248f48be66a13c7bb7387d9db44fc44f51f1))


### Bug Fixes

* **release:** pass on outputs from "release-please" to workflow ([0831598](https://github.com/ZeitOnline/gh-action-workflows/commit/083159842190e4e59303ccaa3552fc4984a25f41))
* **release:** try to fix conditionals ([5e6f320](https://github.com/ZeitOnline/gh-action-workflows/commit/5e6f3208b992c07ec9106555190acbd4f4004fe2))


### Miscellaneous Chores

* remove no longer needed workflows ([aa72c2f](https://github.com/ZeitOnline/gh-action-workflows/commit/aa72c2f2c3c3576f8c0a9ee288aad6a44c4811ad))
