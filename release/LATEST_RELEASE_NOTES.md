# aeval v0.1.2

Built: 2026-03-30 17:42:16 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 0084089f6180f3d8df4be64a76712a9a1695875130cb0b3a37cb077cfa3dbeba | 240081952 |
| aeval-macos-x86_64 | 3cc48ed35573263140265a220acd8cb8c0688089d7ffec0eaf9d2de7a8d7c44a | 142162688 |
| aeval-linux-x86_64 | 4b7a129a1553bfd11183db1808cbb6c6831e5d8927a128ec695ea5256e97bd92 | 407958432 |
| aeval-linux-arm64 | d375414b5f09142610bc6c9ef995964c6fe31035d49a6657c9b8506f00fd07f3 | 359427208 |

## Public Repository Updates

- Updated runtime `config/`
- Updated public `examples/` (interrupt, response, multi_turn_dialogue)
- Updated required `corpus/` subset for those examples
- Updated `release/` notes and checksums
- Preserved existing root `README.md`

## Smoke Test Status

- macOS arm64: skipped
- macOS x86_64: skipped
- Linux x86_64: skipped
- Linux arm64: skipped

## Packaged Binary Validation Status

- Plugin/stage checks on packaged binaries (macOS + Linux): skipped

## Release Scenario Test Status

- Linux x86_64 examples/config runtime: skipped
- Linux arm64 examples/config runtime: skipped

## One-Line Install (Latest Release)

```bash
OS="$(uname -s | tr '[:upper:]' '[:lower:]')"; ARCH="$(uname -m)"; case "${OS}-${ARCH}" in darwin-arm64) TARGET="macos-arm64" ;; darwin-x86_64) TARGET="macos-x86_64" ;; linux-x86_64) TARGET="linux-x86_64" ;; linux-aarch64|linux-arm64) TARGET="linux-arm64" ;; *) echo "Unsupported platform: ${OS}-${ARCH}" >&2; exit 1 ;; esac; curl -fsSL "https://github.com/Agora-Build/aeval/releases/latest/download/aeval-${TARGET}" -o aeval && chmod +x aeval && ./aeval --help
```

## Links

- Releases: https://github.com/Agora-Build/aeval/releases
- Repo: https://github.com/Agora-Build/aeval
