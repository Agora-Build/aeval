# aeval v0.5.3

Built: 2026-11-27 08:45:46 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | f36c241a656d430ecca9c10a47b02698a6827b4893b8fe96ee493d3486a90c39 | 259594656 |
| aeval-macos-x86_64 | f04016d38884945cc1f237fde81dc10466e70e49dec6f67f8631874841a240e2 | 354322720 |
| aeval-linux-x86_64 | c8da726b9c1df3bd9676b76ff6bd9e47310e80d791887d188b8a82d74373c203 | 431757576 |
| aeval-linux-arm64 | c3af9346f9213718cec85e840776a47943b1c34ee27941b1e7848d1739d823cd | 343255656 |


## Public Repository Updates

- Updated root `README.md` with virtual-soundcard setup and audio I/O behavior
- Updated runtime `config/`
- Updated public `examples/` (interrupt, response, multi_turn_dialogue)
- Updated required `corpus/` subset for those examples
- Updated `release/` notes and checksums

## Audio I/O

- Built-in platform configurations default to native virtual-soundcard audio I/O
- Existing scenario actions remain compatible: `audio.play` routes through the resolved audio I/O mode
- macOS requires BlackHole 2ch, BlackHole 16ch and a validated 48 kHz Aggregate Device; Linux requires the `VirtualAudio` `snd-aloop` route
- A scenario can explicitly select the legacy route with `audio_io.mode: web_hook`

## Browser Assistance

- Opt in with `--browser-control-socket`; optional `--session-id` shares execution identity across reports, assistance and automatic private checkpoints
- A private local socket supports on-demand headless browser viewing and explicit input control
- Verification links can open in the same browser context; ordinary continuation is read-only, and credential stages after verification require explicit stage-bound authorization
- Assistance holds are bounded by `--browser-assistance-timeout`; viewers expire after five seconds without heartbeats
- Trial reports include `browser_observation.mode` only when assistance is used

## Smoke Test Status

- macOS arm64: pass
- macOS x86_64: pass
- Linux x86_64: pass
- Linux arm64: pass
- Real providers on host browser: pass
- Real providers in Docker/headless Linux: pass (target: linux-arm64, platforms: livekit)
- Prerecorded response/interruption analysis in Docker: pass (selected Linux targets)

## Packaged Binary Validation Status

- Plugin/stage checks on packaged binaries (macOS + Linux): pass

## Example Packaging Scan (Not Conversation Qualification)

- Linux x86_64 examples/config runtime: pass
- Linux arm64 examples/config runtime: pass

## One-Line Install (Latest Release)

```bash
OS="$(uname -s | tr '[:upper:]' '[:lower:]')"; ARCH="$(uname -m)"; case "${OS}-${ARCH}" in darwin-arm64) TARGET="macos-arm64" ;; darwin-x86_64) TARGET="macos-x86_64" ;; linux-x86_64) TARGET="linux-x86_64" ;; linux-aarch64|linux-arm64) TARGET="linux-arm64" ;; *) echo "Unsupported platform: ${OS}-${ARCH}" >&2; exit 1 ;; esac; curl -fsSL "https://github.com/Agora-Build/aeval/releases/latest/download/aeval-${TARGET}" -o aeval && chmod +x aeval && ./aeval --help
```

## Links

- Releases: https://github.com/Agora-Build/aeval/releases
- Repo: https://github.com/Agora-Build/aeval

## ElevenLabs Preview

- Dismiss ElevenLabs popups with a `Maybe later` or `Close` button during
  guarded clicks, including starting and ending a call. The rules work when
  WebRTC setup is skipped, do not dismiss alerts or credential prompts, and
  do not run in the background during the evaluation.
- Source installs require Playwright 1.44 or later for scoped dialog handling.

## Native False-Interruption Scoring

- Score native `INT_FALSE` recordings on the WAV timeline instead of requiring
  a browser clock. Keep the 1,500 ms agent speech-resume rule. `web_hook` runs
  still use the browser-timed preset and its clock checks.
- Use the native preset for a consistent comparison baseline: playback windows
  can associate speech with different turns than the browser-timed preset did.
