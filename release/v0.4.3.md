# aeval v0.4.3

Built: 2026-10-29 22:42:44 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 97d2b268a4fcbaab058a455818a7a2499de1ccb2800b2273a1e42adf8d4faae1 | 259473648 |
| aeval-macos-x86_64 | 01d2e464e60af189caa92ae69698b5d688dad0465d122461e9c551e88380bf81 | 354201424 |
| aeval-linux-x86_64 | 26f5eb8ba79a6503d623e5a661b21b3089e6c4171da1be79ef546a051cc16418 | 431636264 |
| aeval-linux-arm64 | 2a9764e54d10d92cb843c56ce9e2c0730a319a99768d0293a207e61575a54ec2 | 343138360 |


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

## Smoke Test Status

- macOS arm64: pass
- macOS x86_64: pass
- Linux x86_64: pass
- Linux arm64: pass
- Real providers on host browser: pass
- Real providers in Docker/headless Linux: pass (platforms: livekit)

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
