# DSPLab Projects — Metadata Schema

Every `.vult` file may have a companion `<filename>.vult.meta` file in the
same directory.  The `.meta` file is a UTF-8 JSON document that describes the
patch or module for display in the DSPLab community browser.

Metadata is **optional but strongly encouraged**.  If no `.meta` file exists
the browser falls back to the filename.

---

## Versioning

The `schema_version` field allows DSPLab to handle future breaking changes
without breaking older files.  Unknown top-level keys are **always ignored**,
so forward-compatible additions (new optional fields) never require a version
bump.  Only removals or semantic changes to existing fields require a bump.

Current stable version: **1**

---

## Full Schema Reference

```jsonc
{
  // ── Identity ─────────────────────────────────────────────────────────────
  "schema_version": 1,          // integer, required. Bump on breaking changes.

  "name": "Moog Ladder Filter", // string, required. Human-readable display name.

  "description": "A self-oscillating 4-pole ladder filter modelled after the
                  classic Moog topology, with tanh saturation in each stage.",
                                // string, required. 1-4 sentences. No markdown.

  "author": "DatanoiseTV",      // string, required. GitHub username or display name.

  "version": "1.0.0",           // string, required. Semver of the patch itself.

  "created_at": "2026-03-09",   // string, required. ISO 8601 date (YYYY-MM-DD).

  "license": "MIT",             // string, required. SPDX identifier.
                                // Common values: "MIT", "GPL-3.0", "CC0-1.0"

  // ── Classification ───────────────────────────────────────────────────────
  "type": "preset",             // string, required. "preset" or "module".
                                // Mirrors folder convention but explicit here.

  "role": "effect",             // string, required for type=="preset". One of:
                                //   "instrument" — generates sound (needs MIDI)
                                //   "effect"     — processes audio input
                                //   "utility"    — metering, routing, analysis
                                // Not used for modules.

  "category": "filter",         // string, required. Fine-grained subcategory.
                                // Preset categories:
                                //   "synthesizer" "filter" "effect" "utility"
                                //   "drum" "sequencer" "modulator"
                                // Module categories:
                                //   "filter" "oscillator" "envelope" "effect"
                                //   "utility" "math" "modulator"

  "tags": ["filter", "analog", "resonant", "self-oscillating"],
                                // array of strings, required (may be empty []).
                                // Lowercase, hyphen-separated. Used for search.

  "complexity": "intermediate", // string, required. One of:
                                //   "beginner" "intermediate" "advanced"

  "inputs": {                   // object, optional.
    "audio":  true,             //   bool: accepts audio input
    "midi":   false,            //   bool: responds to noteOn/noteOff/CC
    "cv":     false             //   bool: intended for CV/modular use
  },

  "knobs": {                    // object, optional. Keys are CC numbers (strings).
    "30": {
      "name":    "Cutoff",      //   string: human-readable parameter name
      "description": "Filter cutoff frequency, normalised 0-1 mapped to 20Hz-20kHz",
      "default": 0.5,           //   number: default value (0.0-1.0 for CC)
      "unit":    "norm"         //   string: "norm" | "hz" | "db" | "semitone"
                                //           | "seconds" | "percent" | "custom"
    },
    "31": {
      "name":    "Resonance",
      "description": "Self-oscillation starts above 0.9",
      "default": 0.1,
      "unit":    "norm"
    }
  },

  // ── Module-specific (type == "module") ───────────────────────────────────
  "functions": [                // array, optional. Public functions exposed.
    {
      "name":        "svf_lp",
      "signature":   "svf_lp(x:real, fc:real, q:real) : real",
      "description": "Lowpass output of the state-variable filter."
    }
  ],

  // ── Future / optional ────────────────────────────────────────────────────
  "min_dsplab_version": "1.0.0", // string, optional. Semver minimum.
  "preview_audio": null,         // string | null. Future: URL to a short demo clip.
  "thumbnail": null,             // string | null. Future: URL to a screenshot/image.
  "dependencies": []             // array of strings. Future: other module paths.
}
```

---

## Preset roles

| Value | Meaning |
|---|---|
| `instrument` | Generates sound, typically driven by MIDI noteOn/noteOff |
| `effect` | Processes an audio input signal |
| `utility` | Signal routing, metering, analysis — no primary audio character |

## Preset categories (fine-grained, within a role)

| Value | Role | Meaning |
|---|---|---|
| `synthesizer` | instrument | Oscillator-based sound generator |
| `drum` | instrument | Percussive sound generator |
| `sequencer` | instrument | Rhythmic or melodic pattern generator |
| `filter` | effect | Processes audio through a filter |
| `effect` | effect | Time-based or non-linear effect (delay, reverb, chorus…) |
| `modulator` | effect | Modifies timbre or dynamics (compressor, waveshaper…) |
| `utility` | utility | Signal routing, mixing, metering |

## Module categories

| Value | Meaning |
|---|---|
| `filter` | Filter building block |
| `oscillator` | Waveform generator |
| `envelope` | Amplitude or parameter envelope |
| `effect` | Effect unit |
| `utility` | General-purpose signal utility |
| `math` | Pure mathematical helper (no audio state) |
| `modulator` | LFO, noise, or other modulation source |

---

## Minimal valid example

```json
{
  "schema_version": 1,
  "name": "One-Pole Filter",
  "description": "Simple one-pole lowpass filter for smoothing or tone shaping.",
  "author": "DatanoiseTV",
  "version": "1.0.0",
  "created_at": "2026-03-09",
  "license": "MIT",
  "category": "filter",
  "tags": ["filter", "lowpass", "simple"],
  "complexity": "beginner",
  "type": "module"
}
```
