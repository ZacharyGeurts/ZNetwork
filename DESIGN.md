# ZNetwork — NewLatest canonical (2.1.0 Stack)

**Verdict: ACTIVE relayer for stack; field stream still lab-gated.**

This tree is the **only canonical copy** — `NewLatest/ZNetwork`. Integrated stacks (`NewLatest/lib/znetwork-*.py`, Grok16 field wire) attach here; no SG sibling duplicate.

## Policy

| Gate | Default | Meaning |
|------|---------|---------|
| `wide_open_forbidden` | `true` | No production field-stream takeover |
| `field_stream_forbidden_until_lab` | `true` | Steady-state no-app-packet stream blocked until lab battery + soak |
| `ZNETWORK_LAB_GATE_OK` | unset | Must be `1` after `scripts/znetwork-lab-gate.sh` passes |
| `ZNETWORK_SHADOW_SOAK_APPROVED` | unset | Must be `1` after 72h SHADOW soak (manual) |
| `field_stream_lab_approved` | `false` | Checklist item; do not set without soak |

See `data/znetwork-outside-lab-doctrine.json` and `data/review-checklist.json`.

## Lab workflow

```bash
cd NewLatest/ZNetwork
./scripts/znetwork-test-battery.sh    # smoke + sims
./scripts/znetwork-lab-gate.sh        # fails closed; exports gate on success
```

Build (host toolchain — avoid g16 linker in PATH):

```bash
cd build && CXX=g++ CC=gcc make clean && CXX=g++ CC=gcc make
```

## SHADOW mode

C++ `mode.cpp`: without `ZNETWORK_LAB_GATE_OK=1`, SHADOW requests downgrade to REVIEW_ONLY with reason `lab_gate_required`.

## Integrated pointers

- `NewLatest/znetwork/OUTSIDE_LAB.json` — pointer only
- `NewLatest/lib/znetwork-field.sh` — resolves `NewLatest/ZNetwork` first
- `Grok16/data/grok16-znetwork-field-wire-doctrine.json` — `field_stream_mode.status`: `lab_locked_not_ready`