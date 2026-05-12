# pi-google-code-execution

[![ci](https://github.com/code-yeongyu/pi-google-code-execution/actions/workflows/ci.yml/badge.svg)](https://github.com/code-yeongyu/pi-google-code-execution/actions/workflows/ci.yml) [![license: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Google native code execution extension for the [pi coding agent](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent).

This package is the standalone extraction of senpi's former builtin `google-code-execution` extension.

## Behavior

The extension does not register a new tool. It intercepts Google provider requests before they are sent and ensures a native `codeExecution` tool is present for `google-generative-ai` / `google-vertex` payloads when code execution is enabled via opt-in env var.

| Case | Result |
|------|--------|
| API is `google-generative-ai` or `google-vertex`, `PI_GOOGLE_CODE_EXECUTION` is truthy, and no Tool object has a `codeExecution` key | injects a new Tool entry `{ codeExecution: {} }` |
| Existing Tool object already has `codeExecution` key (any value) | preserves caller payload without duplication or overwrite |
| Existing Tool object has only `functionDeclarations` | keeps that object and adds separate `{ codeExecution: {} }` entry |
| `PI_GOOGLE_CODE_EXECUTION` is unset/empty/falsy/unknown | no-op |
| API is non-Google | no-op |

Truthy values for `PI_GOOGLE_CODE_EXECUTION` are: `1`, `true`, `yes`, `on` (case-insensitive, surrounding whitespace allowed). Any other value disables injection (opt-in default off).

It also appends a system-prompt section for Google sessions indicating native `code_execution` availability when enabled.

## Installation

The package targets the [`pi`](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent) coding agent. Pi loads extensions from `~/.pi/agent/extensions/`, project `.pi/extensions/`, or via the `--extension` / `-e` CLI flag.

```bash
# From npm (once published)
pi install npm:pi-google-code-execution

# From git
pi install git:github.com/code-yeongyu/pi-google-code-execution

# Manual placement
git clone https://github.com/code-yeongyu/pi-google-code-execution ~/.pi/agent/extensions/pi-google-code-execution
cd ~/.pi/agent/extensions/pi-google-code-execution && npm install

# Dev / one-shot test
pi -e /path/to/pi-google-code-execution/src/index.ts
```

After installation, restart pi or run `/reload` inside an interactive session.

## Development

```bash
npm install
npm test
npm run typecheck
npm run check
pi -e ./src/index.ts
```

The test suite uses vitest. TypeScript is strict, Node-only, and uses ESM imports with `.js` suffixes.

## Origin

Ported from `packages/coding-agent/src/core/extensions/builtin/google-code-execution/index.ts` in the senpi-mono fork of pi-mono.
