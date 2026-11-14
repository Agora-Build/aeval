# aeval v0.4.5

Built: 2026-11-14 04:43:43 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 356bd285b20eccb3874f9c0277b60e484889cd57c9b0ad89becc3e6d8ac7388a | 259514432 |
| aeval-macos-x86_64 | 04f7fd9238dfe4648db2537de0ac84ee1464e9b1f7b2b0acd83b72acfd867466 | 354242176 |
| aeval-linux-x86_64 | 113f6dc66baf59bda0463bc3da30157d5c31800366a19db87df189024e8ea0d5 | 431677456 |
| aeval-linux-arm64 | f9d5585ae6956a5b06cb96841c0f977b519ea72c50974f6293b54294062c150a | 343176800 |


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

- Plugin/stage checks on packaged binaries (macOS + Linux): pass

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
