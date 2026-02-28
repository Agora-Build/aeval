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
