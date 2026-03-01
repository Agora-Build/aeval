# aeval v0.1.0

Built: 2026-03-01 04:32:30 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 26ae20ae6eb48841a652b3f22312e0c831d88e3c59b2eb7c036b84cca6a7a940 | 239537296 |
| aeval-macos-x86_64 | d6ce84f4a5e15dac82a59b6cf2375d339dc35e35552b78be93a0a03b18fec443 | 141576320 |
| aeval-linux-x86_64 | 9f633e51c3987325279f017f8bac15a839f64f20bf565708a9fb4cb3e79de2f4 | 180639384 |
| aeval-linux-arm64 | 60519f9fdf8396d6965fab5bdc956d37a4e272615a0ed8d592ebd6aef1bf0ceb | 174334352 |

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
