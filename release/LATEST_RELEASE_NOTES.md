# aeval v0.5.4

Built: 2026-11-28 12:55:55 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 408fbfec9d843cc16d69d83b0c10fdf519ff9b651774a1062bc6e06232ec2095 | 259596816 |
| aeval-macos-x86_64 | 218f20a703a82231cff724beb6a93f7790b3e986163ddfb71084583ca5ca7a0e | 354324784 |
| aeval-linux-x86_64 | 2307c3c456e77cb60bbcf02995beb23214a5ae98326f3d781f516e8a0bf42ecb | 431758096 |
| aeval-linux-arm64 | a9099fe3a840ffcfa249e8de4b801205b3a9065ef973389962ce8c040d953e24 | 343257920 |


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

## ElevenLabs Popups

- Guarded clicks can dismiss stacked eligible popups one at a time, without
  relying on promotion copy. Each popup must disappear before dismissal
  continues, and the original click timeout still applies.
- Log visible dialog counts, matching dismiss buttons, and dismissal timeouts
  in the session's `logs/convo_benchmark.log` to diagnose failures like #40882.
- Alerts and credential prompts are still excluded. Pages with no eligible
  popups continue to use the ordinary click path.
