# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.1] - 2026-09-24

### Changed

- Refresh dependencies to latest: @biomejs/biome 2.5.14, typescript 7.0.2, vitest 5.0.1, @types/node 26.6.2.
- Migrate peer dependencies from @mariozechner/pi-* to @earendil-works/pi-* (0.87.1).
- Update Node.js engine requirement to >=22.19.0.
- Update CI to Bun 1.4.2 with matrix testing on Node 22 and 24, Ubuntu and macOS.
- Remove npm registry install path from README (git:// distribution only).

## [0.1.0] - 2026-05-07

### Added

- Initial release. Native Google `codeExecution` policy extension for the pi coding agent. Injects `{ codeExecution: {} }` into `google-generative-ai` and `google-vertex` requests when `PI_GOOGLE_CODE_EXECUTION` is enabled (opt-in).
