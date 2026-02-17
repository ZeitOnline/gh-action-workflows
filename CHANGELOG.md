# Changelog

## [2.11.3](https://github.com/ZeitOnline/gh-action-workflows/compare/2.11.2...2.11.3) (2026-02-17)


### Bug Fixes

* **linter:** don't install Node.js by default after all ([#91](https://github.com/ZeitOnline/gh-action-workflows/issues/91)) ([0bc708a](https://github.com/ZeitOnline/gh-action-workflows/commit/0bc708a86e3c86d97601a3e6063a5b3e96684843))

## [2.11.2](https://github.com/ZeitOnline/gh-action-workflows/compare/2.11.1...2.11.2) (2026-02-17)


### Bug Fixes

* **lefthook:** use `setup-command` to install tools (via `uv`) ([#89](https://github.com/ZeitOnline/gh-action-workflows/issues/89)) ([2a117fa](https://github.com/ZeitOnline/gh-action-workflows/commit/2a117fa005f5e88980fb7c3736fd7a64aa965818)), closes [#87](https://github.com/ZeitOnline/gh-action-workflows/issues/87)

## [2.11.1](https://github.com/ZeitOnline/gh-action-workflows/compare/2.11.0...2.11.1) (2026-02-12)


### Bug Fixes

* **lefthook:** switch to `uv run` and allow passing extra options ([#87](https://github.com/ZeitOnline/gh-action-workflows/issues/87)) ([503741b](https://github.com/ZeitOnline/gh-action-workflows/commit/503741b4600e8fa4027ebba7fc006aff51cb418b))

## [2.11.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.10.1...2.11.0) (2026-01-22)


### Features

* **linter:** add workflow to run 'pre-commit' checks with 'lefthook' ([#80](https://github.com/ZeitOnline/gh-action-workflows/issues/80)) ([008fb5b](https://github.com/ZeitOnline/gh-action-workflows/commit/008fb5b7eee1a9a374c15c4049ece4218b4fb6c8))

## [2.10.1](https://github.com/ZeitOnline/gh-action-workflows/compare/2.10.0...2.10.1) (2025-12-04)


### Bug Fixes

* **nightwatch:** handle duplicate, but identical image names ([5ebbfd5](https://github.com/ZeitOnline/gh-action-workflows/commit/5ebbfd57f34983593f9eccf627e6024cccacc11e)), closes [#70](https://github.com/ZeitOnline/gh-action-workflows/issues/70)

## [2.10.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.9.0...2.10.0) (2025-12-02)


### Features

* **nightwatch:** read image name from project kustomization ([#70](https://github.com/ZeitOnline/gh-action-workflows/issues/70)) ([3c04aba](https://github.com/ZeitOnline/gh-action-workflows/commit/3c04abaeeb2ecefac784fcb8f2f86680204acd5e))

## [2.9.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.8.0...2.9.0) (2025-11-26)


### Features

* **nightwatch:** pass cluster to baseproject action ([#68](https://github.com/ZeitOnline/gh-action-workflows/issues/68)) ([1fcc48b](https://github.com/ZeitOnline/gh-action-workflows/commit/1fcc48b5389e635ae3cb620feefc0f56112b1aff))

## [2.8.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.7.0...2.8.0) (2025-10-17)


### Features

* install & run 'pre-commit' using `uv(x)` ([#56](https://github.com/ZeitOnline/gh-action-workflows/issues/56)) ([9d4de6a](https://github.com/ZeitOnline/gh-action-workflows/commit/9d4de6a8c4d32cf9118412c915aaa55f0e94f110))


### Bug Fixes

* **k8s-validation:** use 'staging' and 'production' by default ([0c6b13e](https://github.com/ZeitOnline/gh-action-workflows/commit/0c6b13ed7b8bab4ecba4b746af12b743d25b0462)), closes [#33](https://github.com/ZeitOnline/gh-action-workflows/issues/33)

## [2.7.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.6.1...2.7.0) (2025-10-17)


### Features

* **k8s-validation:** add input to be flexible which environments to validate ([#33](https://github.com/ZeitOnline/gh-action-workflows/issues/33)) ([38cfaa9](https://github.com/ZeitOnline/gh-action-workflows/commit/38cfaa937e982a51ccf1adb6aa43be8c710b0834))


### Bug Fixes

* **pre-commit:** make pre-commit actually use the node we just installed, sigh ([#57](https://github.com/ZeitOnline/gh-action-workflows/issues/57)) ([5245d81](https://github.com/ZeitOnline/gh-action-workflows/commit/5245d81a34fa5a3193bb1f022bc31d01a4aec5fb))

## [2.6.1](https://github.com/ZeitOnline/gh-action-workflows/compare/2.6.0...2.6.1) (2025-10-02)


### Bug Fixes

* **pre-commit:** show diff on failure to aid debugging ([6dda9aa](https://github.com/ZeitOnline/gh-action-workflows/commit/6dda9aa7e9f44d471149951b202884a3e3a77fc2))

## [2.6.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.5.0...2.6.0) (2025-10-02)


### Features

* **pre-commit:** support installing dependency runtimes (python, nodejs) ([#51](https://github.com/ZeitOnline/gh-action-workflows/issues/51)) ([784ba2f](https://github.com/ZeitOnline/gh-action-workflows/commit/784ba2fc2056ffa05642b9a3eb67b2644882c8a1))

## [2.5.0](https://github.com/ZeitOnline/gh-action-workflows/compare/2.4.2...2.5.0) (2025-08-29)


### Features

* **commit-lint:** add inputs to commit-lint workflow ([#48](https://github.com/ZeitOnline/gh-action-workflows/issues/48)) ([db20672](https://github.com/ZeitOnline/gh-action-workflows/commit/db20672a26f1c809d1710d4b98632d2c48385d62))

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
