# aeval

Prebuilt `aeval` binaries and runnable example scenarios.

## Download

Release page:

https://github.com/Agora-Build/aeval/releases

Current binary assets:

- `aeval-macos-arm64`
- `aeval-macos-x86_64`
- `aeval-linux-x86_64`
- `aeval-linux-arm64`
- `aeval_SHA256SUMS.txt`

## Install

### Install (Latest Release)

```bash
OS="$(uname -s | tr '[:upper:]' '[:lower:]')"; ARCH="$(uname -m)"; case "${OS}-${ARCH}" in darwin-arm64) TARGET="macos-arm64" ;; darwin-x86_64) TARGET="macos-x86_64" ;; linux-x86_64) TARGET="linux-x86_64" ;; linux-aarch64|linux-arm64) TARGET="linux-arm64" ;; *) echo "Unsupported platform: ${OS}-${ARCH}" >&2; exit 1 ;; esac; curl -fsSL "https://github.com/Agora-Build/aeval/releases/latest/download/aeval-${TARGET}" -o aeval && chmod +x aeval && ./aeval --help
```

### Install (Specific File)

Example for macOS Intel:

```bash
mv aeval-macos-x86_64 aeval
chmod +x aeval
./aeval --help
```

Use the matching binary name for your platform.

## Run Examples

Examples are under `examples/`:

- `examples/interrupt/`
- `examples/response/`
- `examples/multi_turn_dialogue/`

Run commands:

```bash
./aeval run examples/interrupt/interrupt_I00_en.yaml --headful
./aeval run examples/response/response_R00_en.yaml --headful
./aeval run examples/multi_turn_dialogue/scenario.yaml --headful
```
