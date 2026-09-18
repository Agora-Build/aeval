# aeval

Prebuilt `aeval` binaries and runnable voice-agent evaluation scenarios.

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

1. Install BlackHole 2ch.
2. In Audio MIDI Setup, configure BlackHole 2ch for 48 kHz and two channels.
3. Allow the terminal or browser process to access the microphone in macOS
   Privacy & Security settings.

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
