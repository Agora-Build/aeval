# aeval

Prebuilt `aeval` binaries and runnable voice-agent evaluation scenarios.

## Features

- Automated voice-agent evaluations with response latency, interruption and
  turn-taking metrics, plus HTML and JSON reports.
- Native virtual-soundcard playback and stereo User/Agent recording on macOS
  and Linux, using existing scenario actions such as `audio.play`.
- Offline analysis of prerecorded stereo conversations, including bundled
  response and interruption samples.
- Opt-in headless browser assistance through a private application-adapter
  socket: on-demand viewing, explicit input control and verification-link navigation.
- Saved account-session validation and reuse, supported multi-stage login,
  and automatic private session snapshots for application-managed persistence.
- Optional caller-supplied `--session-id` shared across reports, artifacts,
  browser assistance and session snapshots.

Version-specific changes and fixes are in the
[release notes](release/LATEST_RELEASE_NOTES.md).

## Download

Download the binary for your operating system and architecture from the
[GitHub releases page](https://github.com/Agora-Build/aeval/releases):

- `aeval-macos-arm64`
- `aeval-macos-x86_64`
- `aeval-linux-x86_64`
- `aeval-linux-arm64`
- `aeval_SHA256SUMS.txt`

## Install

Install the latest release automatically:

```bash
OS="$(uname -s | tr '[:upper:]' '[:lower:]')"; ARCH="$(uname -m)"; case "${OS}-${ARCH}" in darwin-arm64) TARGET="macos-arm64" ;; darwin-x86_64) TARGET="macos-x86_64" ;; linux-x86_64) TARGET="linux-x86_64" ;; linux-aarch64|linux-arm64) TARGET="linux-arm64" ;; *) echo "Unsupported platform: ${OS}-${ARCH}" >&2; exit 1 ;; esac; curl -fsSL "https://github.com/Agora-Build/aeval/releases/latest/download/aeval-${TARGET}" -o aeval && chmod +x aeval && ./aeval --help
```

For a manually downloaded file, rename it to `aeval`, make it executable, and
verify it:

```bash
mv aeval-macos-arm64 aeval
chmod +x aeval
./aeval --version
```

## Audio I/O

Native virtual-soundcard audio I/O is the runtime default, even when `audio_io`
is omitted. Existing scenarios remain unchanged: `audio.play` automatically
uses the resolved soundcard route for corpus playback. The default device name
is `BlackHole 2ch` on macOS and the ALSA card ID is `VirtualAudio` on Linux.
Set `audio_io.soundcard.device` to override the device label or card ID.

### macOS

1. Install BlackHole 2ch (user input) and BlackHole 16ch (agent output).
2. Configure both for 48 kHz and create an Aggregate Device containing only
   these two devices. Use BlackHole 2ch as the clock and enable drift correction
   for BlackHole 16ch. This lets the native recorder capture both sides on one
   timeline without mixing them.
3. Allow the terminal and Chromium to access the microphone in macOS
   Privacy & Security settings.

With [audictl](https://github.com/Agora-Build/audictl), create the route without
changing the system's default input/output:

```bash
audictl aggregate create --name Aeval_Smoke_Tests_Device \
  --devices 'BlackHole 2ch,BlackHole 16ch' --clock 'BlackHole 2ch' \
  --drift 'BlackHole 16ch'
audictl rate set Aeval_Smoke_Tests_Device 48k
audictl aggregate show Aeval_Smoke_Tests_Device --json
```

Save a host profile, for example `~/.config/aeval/smoke-tests-device.yaml`, using the
aggregate UID reported by `audictl` (not its numeric device ID):

```yaml
aggregate_device: Aeval_Smoke_Tests_Device
aggregate_uid: audictl-REPLACE-WITH-YOUR-AGGREGATE-UID
user_uid: BlackHole2ch_UID
agent_uid: BlackHole16ch_UID
agent_output: BlackHole 16ch
```

Select it once for the host, without editing existing scenarios:

```bash
export CONVOBENCH_SOUNDCARD_ROUTE_PROFILE="$HOME/.config/aeval/smoke-tests-device.yaml"
```

An explicit `audio_io.soundcard.route_profile` takes precedence. aeval validates
the UID, membership, channel order and drift correction before opening Chromium;
an invalid route fails instead of falling back to browser-injected audio.

macOS microphone authorization is separate from Chromium's website permissions.
aeval checks the launching process's authorization before native audio setup and
fails with `SOUNDCARD_BROWSER_PERMISSION` if access is denied, restricted, or not
yet granted. A live BlackHole track and a successful playback call do not prove
that microphone samples reached the browser.

For SSH runs, macOS may attribute microphone access to Remote Login
(`sshd-keygen-wrapper`), rather than Chromium. A background SSH session cannot
display the system consent dialog. Grant access from a local GUI session, or
run from an already authorized local terminal, then rerun manually. Do not reset
privacy settings or change TCC databases to work around this error.

### Linux

Install the host libraries, load `snd-aloop` with the required stable card ID,
and add the current user to the audio group:

```bash
sudo apt-get update
sudo apt-get install -y alsa-utils libportaudio2 libsndfile1
sudo modprobe snd-aloop id=VirtualAudio pcm_substreams=1
sudo usermod -aG audio "$USER"
```

Log in again after changing group membership. Before running `aeval`, verify
that `/proc/asound/cards`, `aplay -l`, and `arecord -l` show the `VirtualAudio`
card with playback and capture devices.

To use the legacy browser-injection route instead, set a scenario-level override:

```yaml
audio_io:
  mode: web_hook
```

## Run Examples

Examples are under `examples/interrupt/`, `examples/response/`, and
`examples/multi_turn_dialogue/`:

```bash
./aeval run examples/interrupt/interrupt_I00_en.yaml --headful
./aeval run examples/response/response_R00_en.yaml --headful
./aeval run examples/multi_turn_dialogue/scenario.yaml --headful
```

Authenticated platforms require the corresponding environment variables or a
saved browser storage state. LiveKit's public example does not require account
credentials.

## Analyze a Prerecorded Conversation

`aeval analyze` can calculate response latency, interruption latency, and
turn-taking metrics from one prerecorded stereo WAV without a scenario or
browser metadata. The channel assignment is required:

- left / channel 0: user
- right / channel 1: agent

Both channels must use the same timeline. Preserve silence and overlapping
speech, and avoid speaker leakage between channels. PCM WAV is recommended;
the analyzer resamples the recording internally. Mono or mixed-down audio
cannot provide reliable speaker-specific latency metrics.

Run the bundled 20-second examples:

```bash
./aeval analyze examples/offline_analysis/response \
  --config config/analysis_presets/offline_stereo.yaml

./aeval analyze examples/offline_analysis/interruption \
  --config config/analysis_presets/offline_stereo.yaml
```

The `response` recording validates response latency and turn-taking. The
`interruption` recording validates interruption action latency and
post-interruption continuation.

To analyze your own recording, place it in any session directory:

```text
offline-session/
`-- recordings/
    `-- conversation.wav
```

Run the shipped event-free preset:

```bash
./aeval analyze offline-session \
  --config config/analysis_presets/offline_stereo.yaml
```

No `console.log`, transcript, or metadata sidecar is needed. The analysis is
written to `offline-session/analysis/`, including `vad.json`, `turns.json`,
`metrics.json`, and `report.html`.

The main JSON metrics are:

- `response_metrics.latency.summary`: agent start minus user end
- `interruption_metrics.latency.summary`: agent stop minus interruption start
- `interruption_metrics.post_interruption_latency.summary`: next agent start
  minus interruption end
- `analysis_quality`: coverage and ambiguity indicators for interpreting the
  measurements

For a session originally produced by `aeval run`, keep its
`recordings/` and `logs/console.log` directories and use the event-aware default
preset instead:

```bash
./aeval analyze SESSION_DIR \
  --config config/analysis_presets/default.yaml
```

## Optional Browser Assistance

An authorized application adapter can inspect and assist a headless run through
an opt-in private local socket:

```bash
./aeval run examples/elevenlabs-agents.yaml \
  --browser-control-socket /tmp/aeval-private-job/control.sock \
  --browser-assistance-timeout 60
```

The socket directory must be private (mode `0700`); it is created if absent.
Keep it outside exported artifacts. This does not expose a public browser link
or include a viewer UI. The application must authorize access and separately
authorize Take Control.

Use the matching current application adapter. There is no client-protocol
launch option or compatibility negotiation; unsupported commands and malformed
or stale messages fail explicitly.

Views start read-only. During supported authentication, element, or URL waits,
an explicit controller can click, type, scroll, and open a verification URL in
the same browser context. Links open in a new tab by default, preserving the
login tab. Continuation restores the original target once and waits for its
configured readiness condition, including delayed page rendering, without
resubmitting login actions or repeatedly reloading the page. Capture/input stop
after five seconds without the viewer, and a held condition is checked before
evaluation can continue. The adapter can poll safe readiness reasons and the
last assistance outcome through `session.status`; a continuation acknowledgment
alone does not confirm readiness.

Adapters can offer provider-configured verification and a single explicit
**Sign in & continue** action. ElevenLabs verification opens in the original
evaluation tab. Additional challenges remain with the user. Adapters can also
open, operate and close an individual manual tab while the evaluation continues.
Manual tabs share the browser's login session and are not credential targets.

The local hold limit defaults to 60 seconds (configurable from 1 to 600). It
does not extend application/job deadlines, and heartbeats cannot extend the
hold. With no socket option, ordinary execution and timeout behavior remain
unchanged. No scenario YAML changes are needed.

## Account Authentication

In ElevenLabs account mode, an explicit `storage_file` is restored and validated
before submitting credentials. Accepted state skips login. Missing, corrupt,
or provider-rejected state falls back to account login within the same setup,
without restarting or replaying evaluation actions. Omitting the file does not
opt into automatic state-file discovery. Storage-only mode has no account-login
fallback. Keep state files private (normally mode `0600`) and outside artifacts.

Recognized combined or sequential email/password forms use the provider's
authentication profile. Verification links and challenges remain human actions.
After human intervention, credential entry requires an explicit command from
the authorized adapter: `authentication.continue` for one stage or
`authentication.continue_flow` for an opted-in provider sequence. Passive
readiness checks and viewer disconnects do not grant authorization. Unsupported
stages require human assistance or fail safely.

Confirmed authentication is saved before checking agent-preview readiness, so a
later preview or evaluation failure does not discard the saved session. Other
providers need reviewed authentication profiles. Capability discovery with
`./aeval capabilities --json` reports development status and qualified providers;
interactive multi-stage verification and application checkpoint consumption
still require deployment-specific qualification.

An explicit account `storage_file` automatically publishes immutable confirmed
session snapshots and ready records beside the working file. For example,
`/private/job-auth/storage.json` uses the private directory
`/private/job-auth/storage.json.checkpoints/` (mode `0700`). The working file
stays at the supplied path, normally mode `0600`. Relative paths and missing
storage parents are supported; existing project permissions are not changed.
State and checkpoints must remain outside artifacts. The application owns
encrypted persistence, receipts and cleanup; aeval does not upload these files.
No separate checkpoint or protocol flags are needed.

Applications and CI may choose one execution identity:

```bash
./aeval run examples/elevenlabs-agents.yaml --session-id ci-example-001
```

The same ID appears in reports, logs, artifact paths, browser assistance and
checkpoint records. Omit the option to retain the automatically generated ID.
Acceptable IDs contain 1-128 ASCII letters/digits, underscores or hyphens and
start with a letter/digit. A supplied ID cannot overwrite an existing execution
directory. Each new execution uses a new ID; saved provider login state can be
reused across IDs. An ID alone enables no persistence and grants no account
authority. The earlier authentication-specific launch options are removed.

## License

The project is licensed under the [MIT License](LICENSE), except for the audio
corpus and its metadata under `corpus/`, which are dedicated to the public
domain under [CC0 1.0 Universal](corpus/LICENSE).
