# ConvoBench Examples

Quick-start examples for running ConvoBench scenarios.

- **Scenario-based** — Full conversation flows (e.g. multi-turn dialogue with tangent and recall).
- **Capability-focused (MediaLab)** — Each targets one Agent capability (e.g. interruption handling, response quality).

---

## Scenario-based examples

| Example                                 | Directory              | Description                                                                   |
|-----------------------------------------|------------------------|-------------------------------------------------------------------------------|
| Multi-turn dialogue (with interruption) | `multi_turn_dialogue/` | 4-turn conversation: Paris → Louvre → off-topic tangent (WiFi) → resume topic |

### Multi-turn dialogue with interruption (`multi_turn_dialogue/`)

A 4-turn **conversation example** that includes an off-topic tangent and context recall:

1. **Turn 1**: General inquiry (Paris travel tips)
2. **Turn 2**: Follow-up (Louvre tickets)
3. **Turn 3**: Off-topic tangent (WiFi password) — dialogue continues
4. **Turn 4**: Apologize and ask to resume previous topic

**Run:**

```bash
convo-bench run examples/multi_turn_dialogue/scenario.yaml
```

**Covers:** multi-turn flow, turn-taking, context maintenance, tangent and recovery.

---

## Capability-focused examples (MediaLab)

Each scenario focuses on **one Agent capability** (interruption or response). Uses corpus from `config/corpus` (e.g.
`convoai.yaml`).

| Capability       | Directory             | Scenario files                      |
|------------------|-----------------------|-------------------------------------|
| **Interruption** | `medialab/interrupt/` | `interrupt_I00_en.yaml` … (7 files) |
| **Response**     | `medialab/response/`  | `response_R00_en.yaml` … (11 files) |

**Run (examples):**

```bash
# Single capability: interruption (e.g. I00 = Quiet-Nonsemantic)
convo-bench run examples/medialab/interrupt/interrupt_I00_en.yaml

# Single capability: response (e.g. R00 = Quiet-Question, 1 round)
convo-bench run examples/medialab/response/response_R00_en.yaml
```

---

## Running examples

- **Scenario-based:** one directory → one main scenario, e.g.
  `convo-bench run examples/multi_turn_dialogue/scenario.yaml`
- **MediaLab:** one scenario per capability variant, e.g.
  `convo-bench run examples/medialab/interrupt/interrupt_I02_en.yaml`

## Adding your own examples

1. Create a new directory under `examples/` (e.g. `examples/my_example/`).
2. Add `scenario.yaml` (and optionally an `audio/` subdirectory for WAV files).
3. Use paths relative to the scenario file or `corpus_id` / `corpus_set` from config.
4. Add a short entry in the table above and a subsection if needed.

See existing examples for reference.
