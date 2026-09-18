# Offline Stereo Analysis Example

The `response/` and `interruption/` directories are independent 20-second
prerecorded sessions for testing `aeval analyze` without a browser session or
event log.

Both recordings are PCM signed 16-bit stereo:

- left / channel 0: user
- right / channel 1: agent

`response` contains two completed user-to-agent exchanges at 44.1 kHz. It
covers response latency and turn-taking analysis; it does not contain
overlapping speech, so its interruption count is expected to be zero.

`interruption` contains overlapping user and agent speech at 48 kHz. It covers
interruption action latency and post-interruption continuation.

Run from the repository root:

```bash
./aeval analyze examples/offline_analysis/response \
  --config config/analysis_presets/offline_stereo.yaml

./aeval analyze examples/offline_analysis/interruption \
  --config config/analysis_presets/offline_stereo.yaml
```

Results are written to the selected session's `analysis/` directory. Generated
analysis and log directories are ignored by Git. Keep additional recordings in
separate session directories so their artifacts do not overwrite each other.
