# aeval v0.5.1

Built: 2026-11-23 07:03:49 UTC

## Binary Assets

| Asset | SHA256 | Size (bytes) |
| --- | --- | ---: |
| aeval-macos-arm64 | 6f5cb2ad8986f68408704abba2a3fa790b6caca91aab15fbeec6cf80a5e3386e | 259589696 |
| aeval-macos-x86_64 | d1ecc015f7795b2074a431d50938193b25a7cf2ad42138f3d21ccb216be98b4c | 354317040 |
| aeval-linux-x86_64 | 197a7a2fd9fb08da41d99f3fe96e0fecca42817daa5c445d0bccd9527136b70e | 431750296 |
| aeval-linux-arm64 | b34a339d3d371c41489e6b2cbee3c3267cefae197695a1723bcc97546308d594 | 343251632 |


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

## Breaking Changes

- Checkpoint and capability identifiers no longer name a product, because the
  packaged `aeval` and the source `convo-bench` write the same formats. Ready
  checkpoint records carry `"schema": "auth.checkpoint.v3"` instead of
  `"protocol": "aeval.auth.v2"`. `capabilities --json` reports the schema
  `eval.capabilities.v1`, the key `authentication.checkpoint_schema` and the
  capability `auth.checkpoint.file.v3`. A record in another format is rejected
  with `AUTH_CHECKPOINT_SCHEMA_UNSUPPORTED`. There are no aliases: applications
  that read checkpoint records must update to these identifiers. Ready records
  already in `<storage_file>.checkpoints/` cannot be read after upgrading; saved
  storage files keep working.

## Fixes

- Replace known credentials in every log line, including browser console
  output: credential-like environment values (`*_EMAIL`, `*_PASSWORD`,
  `*_TOKEN`, `*_SECRET`, `*_API_KEY`) and values used by login steps. Mask
  credentials in dict, JSON, quoted and `Bearer` text, and stop printing local
  variable values in tracebacks.
- ElevenLabs login recognises the email-only sign-in page only when its Sign in
  button is present. The email-verification page keeps the email field beside
  Resend; it is no longer treated as a credential stage, so login no longer
  clicks Resend or times out on it and leaves the page for the human.
- Account login: a timeout after rejecting a wrong saved account now stops for
  assistance instead of reporting `AUTH_BROWSER_OPERATION_FAILED`. A step without
  `authenticated_selector` uses the provider profile's selector. Unexpected
  browser failures log their exception type, never their message. A checkpoint
  that cannot be published still refreshes the storage file unless the path is
  unsafe. Generic login step keys that a provider profile ignores produce a
  warning naming them.
- Browser assistance: signing in to the wrong account while assisting is
  reported as `account_mismatch` and checking continues, instead of expiring
  silently. Failed readiness checks log their exception type once per wait. A
  viewer whose page switch fails is closed so it can reopen.
- Native recording: a recording left running by a failed test, or by a scenario
  without `audio.stop_recording`, is stopped and attached to that test, so later
  tests in a loop can record. An embedded frame that reloads or disappears no
  longer invalidates the recording clock. A recorder shutdown failure no longer
  skips releasing the device lock and the Linux ALSA route.
- macOS: when macOS has never asked the app running ConvoBench for microphone
  access, a desktop run shows the system prompt once and waits up to 60 seconds.
  Denied or restricted access and SSH sessions fail as before.
- Runs started in the same second get distinct session IDs and directories
  instead of sharing one.
- Speech-wait evidence reports each phase's own time budget.
- When only the browser clock link of a valid native recording is invalid, the
  warning now says that recording-timeline analysis (the `native_loopback`
  preset) still scores it, instead of claiming that scoring is disabled.

## Documentation

- The README prerequisites list the full macOS native setup: BlackHole 2ch and
  16ch, a 48 kHz Aggregate Device and its route profile.
- Document the `storage_file` path rule: every folder must be a real folder,
  so not `/tmp` or `$TMPDIR` on macOS.

## Qualification Scope

On November 21, before packaging, source checks passed on macOS arm64: the
full local suite, the native route check, all ten provider-free Chromium
Agent-output hardware cases, and native LiveKit, Agora and ElevenLabs smokes.
Each smoke validated its recording, captured User speech and detected both
speech waits. None of the configured account credentials appeared in full in any
smoke artifact. Source tests do not qualify rebuilt release binaries; the
prepared notes' Smoke Test Status and Packaged Binary Validation Status sections
record the gate results for these binaries.

The checkpoint identifier change has not been qualified with an application
that consumes checkpoint records. Live human-verification stages still need
deployment-specific qualification.
