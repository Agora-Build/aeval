# aeval v0.1.3

Built: 2026-04-02 11:03:55 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 3d0103055c39a96be41865cd2adc6d20ed299e5c381ef2e895a4aca4c187d5df | 259109840 |
| aeval-macos-x86_64 | de46bd40970a278a1c93751eeb1ef5e84e10199ee5a01b90ef879dba138f81f4 | 350685312 |
| aeval-linux-x86_64 | 381937f4ab82d7f99db756ff37ed3296a4c3f3f67b8cacc7ac3847a76ca8d17a | 432299080 |
| aeval-linux-arm64 | b0e8cf29b4eac6f3d75dd2a10bb4c852caa67085a1ce7276cf6297b144530ffe | 380969136 |


## Public Repository Updates

- Updated runtime `config/`
- Updated public `examples/` (interrupt, response, multi_turn_dialogue)
- Updated required `corpus/` subset for those examples
- Updated `release/` notes and checksums
- Preserved existing root `README.md`

## Smoke Test Status

- macOS arm64: pass
- macOS x86_64: pass
- Linux x86_64: pass
- Linux arm64: pass

## Packaged Binary Validation Status

- Plugin/stage checks on packaged binaries (macOS + Linux): pass

## Release Scenario Test Status

- Linux x86_64 examples/config runtime: pass
- Linux arm64 examples/config runtime: pass

## One-Line Install (Latest Release)

```bash
OS="$(uname -s | tr '[:upper:]' '[:lower:]')"; ARCH="$(uname -m)"; case "${OS}-${ARCH}" in darwin-arm64) TARGET="macos-arm64" ;; darwin-x86_64) TARGET="macos-x86_64" ;; linux-x86_64) TARGET="linux-x86_64" ;; linux-aarch64|linux-arm64) TARGET="linux-arm64" ;; *) echo "Unsupported platform: ${OS}-${ARCH}" >&2; exit 1 ;; esac; curl -fsSL "https://github.com/Agora-Build/aeval/releases/latest/download/aeval-${TARGET}" -o aeval && chmod +x aeval && ./aeval --help
```

## Links

- Releases: https://github.com/Agora-Build/aeval/releases
- Repo: https://github.com/Agora-Build/aeval
