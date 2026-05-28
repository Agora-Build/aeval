# aeval v0.2.0

Built: 2026-05-28 11:12:03 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 4282d42aa808d6254748a6b1a25395fd03caeb40e304d0d25a72aa65bc41ccee | 259141024 |
| aeval-macos-x86_64 | f1f14bb0cf26b1a52b028fe71fd2f28fa759ce6a3f24d9e6fbfee46bca546fb5 | 351930736 |
| aeval-linux-x86_64 | bf4116a63840ab574989ef00d0de964218d399f475dfe165315df082b8bc7a25 | 435788744 |
| aeval-linux-arm64 | ba77ba6d1e8c752ea5e229abfa049406db1d28905e2bf804ce9a56c2ff66333d | 384435240 |


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
