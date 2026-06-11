# aeval v0.1.4

Built: 2026-06-11 23:16:03 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 0d8524146090213b8468ed2e6ad08fa3c361f4d44e4abeb618c9321b2d43ab28 | 259140784 |
| aeval-macos-x86_64 | 6fcb4f9577197ee701788c96da5a31b095d03ae5cd0c4b95528a4ac4a9251255 | 352412256 |
| aeval-linux-x86_64 | 4f714a87e7fa7eadbe780824988cc576091052f95ac437b66520083f035a5d41 | 436284376 |
| aeval-linux-arm64 | dfcd229cf2f3814adbca7a217576f13f4b48ac56071807188e70a97006e2d54d | 384919784 |


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
