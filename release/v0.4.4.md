# aeval v0.4.4

Built: 2026-11-13 07:55:55 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 18536d13abd327d420a582208b7960bb35489673cc2b8a0c40010b51dfd14d6d | 259511648 |
| aeval-macos-x86_64 | 506db7157e525b2da735563321cd29226bef9706724a2fa8fb8c07be20dd0eda | 354240064 |
| aeval-linux-x86_64 | a4e1a1dd1b80bd4db6cb2569a99259da5b26860d833601c1be07a750d682a697 | 431676472 |
| aeval-linux-arm64 | 84921aeafc976cf7e079f7937c31bfa9e541d27306a0576135c2af60f4c26e64 | 343173992 |


## Public Repository Updates

- Updated root `README.md` with virtual-soundcard setup and audio I/O behavior
- Updated runtime `config/`
- Updated public `examples/` (interrupt, response, multi_turn_dialogue)
- Updated required `corpus/` subset for those examples
- Updated `release/` notes and checksums

## Audio I/O

- Built-in platform configurations default to native virtual-soundcard audio I/O
- Existing scenario actions remain compatible: `audio.play` routes through the resolved audio I/O mode
- macOS soundcard mode requires BlackHole 2ch at 48 kHz; Linux requires the `VirtualAudio` `snd-aloop` route
- A scenario can explicitly select the legacy route with `audio_io.mode: web_hook`

## Browser Assistance

- Opt in with `--browser-control-socket`; without this flag, execution is unchanged
- A private local socket supports on-demand headless browser viewing and explicit input control
- Verification links can open in the same browser context; authentication continuation checks the protected target without resubmitting credentials
- Assistance holds are bounded by `--browser-assistance-timeout`; viewers expire after five seconds without heartbeats
- Trial reports include `browser_observation.mode` only when assistance is used

## Dependency Updates

- Updated `edge-tts` to 7.3.1 for Microsoft's newer authentication token support, fixing HTTP 403 failures during speech synthesis

## Smoke Test Status

- macOS arm64: skipped
- macOS x86_64: skipped
- Linux x86_64: skipped
- Linux arm64: skipped
- Real providers on host browser: skipped
- Real providers in Docker/headless Linux: skipped

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
