# aeval v0.1.1

Built: 2026-03-03 16:39:57 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 958e6797694a2d11cf2fe283ade35ed3da4992368a46612d4579b675b6629e15 | 239547312 |
| aeval-macos-x86_64 | 53525942b35f38bb79493f64182fb4c8763a4787cd6ac052906fea9d53a904f1 | 141575424 |
| aeval-linux-x86_64 | 66aa14a1ab0f7e5f2153f8d3b6ee87fdbc5f2f6582c27435eac19f05f765c149 | 180646504 |
| aeval-linux-arm64 | 618447154512fabfd342773f6ecfa62b6d1d07be04d0cac6f91924d7b42cfb72 | 174342808 |

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
