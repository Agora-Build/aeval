# ConvoBench Examples

Quick-start examples for running ConvoBench scenarios.

## Directory Layout

- `interrupt/`: interruption capability scenarios
- `response/`: response capability scenarios
- `multi_turn_dialogue/`: multi-turn dialogue scenario with tangent and recall

## Scenario-Based Example

### Multi-turn dialogue (`multi_turn_dialogue/`)

A 4-turn conversation example with off-topic interruption and context recall.

Run:

```bash
convo-bench run examples/multi_turn_dialogue/scenario.yaml
```

## Capability-Focused Examples

### Interruption (`interrupt/`)

Includes 7 scenarios:

- `interrupt_I00_en.yaml`
- `interrupt_I01_en.yaml`
- `interrupt_I02_en.yaml`
- `interrupt_I04_en.yaml`
- `interrupt_I05_en.yaml`
- `interrupt_I06_en.yaml`
- `interrupt_I07_en.yaml`

Run:

```bash
convo-bench run examples/interrupt/interrupt_I00_en.yaml
```

### Response (`response/`)

Includes 10 scenarios:

- `response_R00_en.yaml`
- `response_R01_en.yaml`
- `response_R02_en.yaml`
- `response_R03_en.yaml`
- `response_R04_en.yaml`
- `response_R13_en.yaml`
- `response_R14_en.yaml`
- `response_R15_en.yaml`
- `response_R16_en.yaml`
- `response_R17_en.yaml`

Run:

```bash
convo-bench run examples/response/response_R00_en.yaml
```

## Adding Your Own Example

1. Create a new directory under `examples/`.
2. Add `scenario.yaml` (and optional `audio/` files).
3. Use paths relative to the scenario file or configured corpus IDs.
4. Add an entry to this README.
