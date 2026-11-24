# TinyVM — a correct tiny stack VM

A correct tiny stack virtual machine for make.fun's
`tiny-stack-vm-01-run-20261122` bounty. It matches the reference VM
exactly, every trap code and the step and output limits included.

## Score

Practice set (40 instances): **40/40, score 1,000,000**. The house
baseline (`plain-vm` 1.0.0) scores 670,000 (20/40).

## What it gets right

- Wrapping 32-bit arithmetic; `Math.imul` for MUL (no float rounding).
- Unsigned `DIVU`, `MODU`, `LTU`; `DIV`/`MOD` with the `-2147483648 / -1`
  edge case; shifts masked to `b & 31`.
- `CALL` (depth 64) and `RET`; `LOAD`/`STORE` bounds 0–255.
- Every trap with its own exit code: 2 underflow, 3 overflow, 5 unknown
  opcode, 6 memory, 7 call depth, 8 empty return, 9 bad pc/immediate,
  10 step limit, 11 output limit.
- `PRINT` (decimal, limit checked before writing), `PUTC` (low byte),
  `IN` (`-1` when input is exhausted), `EXIT` (`v & 255`).

## Run it

```sh
MAKEFUN_INPUT_DIR=<input-dir> MAKEFUN_OUTPUT_DIR=<output-dir> node main.ts
```

Each `<id>/` holds `program.bin` and `input.bin`; the run writes
`<id>/stdout.bin` and `<id>/exit.txt`. Node 22, stdlib only.

## Files

`main.ts` — the VM. `LICENSE` — MIT. `owner.json` — wallet.

