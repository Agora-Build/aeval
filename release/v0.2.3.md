# aeval v0.2.3

Built: 2026-06-25 23:53:08 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 6d8acd41327d51f2f589415730e701a9d86192069672c793902e04cb52001063 | 259328560 |
| aeval-macos-x86_64 | 13864ad2283dc1fe6b3835713200284b1f6e81d77144560e355e218f647e1413 | 352550704 |
| aeval-linux-x86_64 | 7ce5d26cc56cfcbf1a7db0a42af9e570b639965cc80f5e0d3ba5df1a4cc9e943 | 436430640 |
| aeval-linux-arm64 | 1383db1e2406cdbe83859f9e6bf9b618f7e580a7ded1916bd929ab3f268cbc74 | 385065984 |


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
