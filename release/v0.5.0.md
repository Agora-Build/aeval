# aeval v0.5.0

Built: 2026-11-20 21:45:29 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 99d7926cef08fd8c9cc7e7009be19c08d141ec3fe9712d18b442052028804452 | 259581680 |
| aeval-macos-x86_64 | 8153d92425b2c073af3a63e65aaf41629422c067aab9f19aefbac3d095885fdf | 354308400 |
| aeval-linux-x86_64 | 38de785f73b827b2197b7ca4b35529a3edd8e4a8d9086687229d7d3e195df0c5 | 431744456 |
| aeval-linux-arm64 | 4178e52361afd798b4841e01355ab0c812146594a2d2bd422d17c6bca54f4783 | 343242800 |


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

## Features

- Generic optional `--session-id` across execution, reports, assistance and
  private session checkpoints, without authentication-specific launch flags.
- Restore-first account authentication for explicit storage files, including
  supported combined and sequential credential stages and stage-bound
  authorization after human verification.
- Automatic private immutable session snapshots and ready records beside the
  explicit storage file, for application-managed encrypted persistence.
- Private headless browser assistance with explicit control, verification URLs,
  bounded readiness continuation and factual observation metadata.
- Host-level macOS aggregate-route selection without changing scenario YAML.
- Separate manual build, prepare and publish commands; publishing verifies the
  exact prepared binaries and notes without rebuilding or rerunning smoke.

## Fixes

- Dispatch frozen multiprocessing workers before CLI imports so packaged native
  recording can start without treating worker arguments as aeval commands.
- Defer media sink binding until playback or explicit output selection so
  discarded preload videos cannot poison native recording startup. Allow muted
  SDK video probes without audio sink selection, and gate subsequent unmute on
  the Agent sink. Genuine routing failures report one contextual diagnostic.
- Preserve synchronous media `src`/`srcObject` reads required by LiveKit and
  other SDKs while continuing to gate playback on the configured Agent output
  device.
- Load remote WebRTC audio only after the element's Agent sink is committed.
  Chromium plays all remote audio in a page through one shared renderer, opened
  on the device of the first element that loads it. Agora's never-played
  per-track elements had moved the Agent to the default output and left native
  recordings silent, while every element still reported the Agent sink.
- Match ElevenLabs' redesigned agent preview call button in account login,
  call start, active-call and exit steps, keeping the previous layout as a
  fallback. Login had stopped recognising the authenticated page and reported
  `AUTH_HUMAN_VERIFICATION_REQUIRED` after its 60-second wait.
- Do not fail native recording for unused permission-restricted embedded
  frames; actual output attempts with invalid routing remain fatal.
- Detect missing macOS microphone authorization before native capture, with
  actionable instructions rather than accepting a silent recording.
- Require actual captured User-channel activity during each prompt in provider
  smoke; playback markers and idle Agent replies alone cannot pass the gate.
- Report the actual speech-wait phase and time budget on timeout.
- Preserve per-input-channel levels and sanitized browser output state in native
  recording manifests to diagnose silent Agent channels without altering PCM,
  role selection or scoring. Add provider-free Chromium Agent-output hardware
  checks for media elements, MediaStreams and Web Audio, using production launch
  settings and verifying that headless Chromium is not muted at launch.
- Include whitelisted inbound WebRTC audio packet/sample counters and decoded
  energy in the bounded stop-time diagnostic snapshot, without exporting SDP,
  network addresses or track IDs, tapping audio, or changing capture validity.
  Correlate each report to the actual player track and include browser-reported
  playout counters; a connection-wide receiver match alone is inconclusive.
- Extend provider-free hardware checks to real WebRTC receiver tracks with the
  full native JavaScript stack and recorder, including headful playback, source
  replacement, and Agora's preset player with a never-played per-track element.
  This covers a different Chromium renderer than local MediaStream tones. An
  integration test also reads Chromium's WebRTC log to check the output device
  of every shared remote-audio renderer, without audio hardware. Real-provider
  qualification remains separate.
- Acquire the pinned ONNX Runtime SDK from Microsoft's official native NuGet
  package instead of the unavailable GitHub SDK archive, and stop cleanly on
  download or package-validation errors.
- Keep native Docker smoke on the soundcard path; missing Linux kernel audio
  devices now fail with setup instructions rather than switching to Web Hook.
- Reject stale packaged versions during release validation and changed
  prepared artifacts during publication.

## Qualification Scope

On November 20, with the remote-audio sink fix, native Agora, ElevenLabs and
LiveKit source smoke runs passed on macOS with full analysis. Each detected both
speech waits and captured Agent audio only on the configured Agent channels. All
ten provider-free Chromium Agent-output hardware cases also passed, including
the Agora-shaped WebRTC case. Source tests do not qualify rebuilt release
binaries. Generated release notes record the actual packaged validation results
and any skipped gates. Offline Docker analysis does not qualify native Linux
audio.

The earlier macOS packaged Agora smoke captured zero signal on the Agent
channels and was rejected by strict validation. With the remote-audio sink fix,
rebuilt macOS arm64 binaries passed the packaged Agora, ElevenLabs and LiveKit
smokes on November 20. ElevenLabs then changed its agent preview page; after
the selector fix, its source smoke passed again. The prepared notes' Smoke
Test Status and Packaged Binary Validation Status sections record the final
gate results for these binaries.

Live human-verification stages and end-to-end application checkpoint consumption
need deployment-specific qualification; LLM-assisted setup recording is not
included in this release.
