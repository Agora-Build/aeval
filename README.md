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

The built-in platform configurations default to native virtual-soundcard audio
I/O. Existing scenarios remain unchanged: `audio.play` automatically uses the
resolved soundcard route for corpus playback.

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
sudo modprobe snd-aloop id=ConvoBench pcm_substreams=1
sudo usermod -aG audio "$USER"
```

Log in again after changing group membership. Before running `aeval`, verify
that `/proc/asound/cards`, `aplay -l`, and `arecord -l` show the `ConvoBench`
card with playback and capture devices.

To use the legacy browser-injection route instead, set a scenario-level
override. The former `audio_input` key is not supported.

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
