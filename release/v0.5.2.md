# aeval v0.5.2

Built: 2026-11-24 03:08:10 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 815e4bcca950cb1cfb8846dcbfd528a39a9bbf9b60971baeca4989d1a2ecc451 | 259592880 |
| aeval-macos-x86_64 | cdb4c94ff27369d19a0f9efd37b3f1d68ce9b92752f122d01d9d36433fcb3869 | 354321456 |
| aeval-linux-x86_64 | d6820292abc5990a71656299b8aae53eef0aa49a8864f816871f35e7863ad67c | 431756944 |
| aeval-linux-arm64 | 53b717d9031f5753bfbb6bd3100685206051a7b7179c5da088241fd6abb8fdd2 | 343254368 |


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

## Browser Assistance

- Add provider-configured verification links and one-click continuation for
  authorized sign-in stages. ElevenLabs opens the link in the original
  evaluation tab; profiles can opt into a tracked second tab. A new human
  challenge, a repeated credential stage or a viewer disconnect stops the flow.
  Existing stage-by-stage authorization remains supported.
- Add manual tabs that share the evaluation's browser context without pausing
  the evaluation or becoming credential targets. Open, operate and close an
  individual manual tab without closing the evaluation or verification tabs.
- Advertise the new socket commands as `auth.verification-flow.v1` and
  `viewer.manual-tabs.v1`. Applications must wire the corresponding controls and
  authorize their use. The public binary does not ship a viewer UI.

## Qualification Scope

Local browser tests use deterministic verification fixtures. They check
same-tab and second-tab flows, readiness on the evaluation target, and stopping
at another challenge. A CLI smoke test uses Chromium and the private socket to
show that the evaluation advances while manual input remains active. These are
distinct from qualification of live human verification through Vox.
