# Cycle-Accurate Out-of-Order Processor Simulator

A C++ simulator of a dynamically scheduled processor over a RISC-V-style integer subset 
(add, addi, sub, mulu, divu), modelling the MIPS R10000 style of out-of-order execution: 
register renaming into a physical register file, an Active List for
in-order commit, and an Integer Queue for wakeup and issue. The simulator reads a program
as JSON and emits the complete architectural and microarchitectural state after every
cycle, so execution can be inspected cycle by cycle rather than only at the end.

- Implements the full pipeline: fetch and decode, rename and dispatch, issue, execute,
  and commit, with the register map table, free list and busy-bit table maintained across
  stages.
- Handles structural hazards and RAW dependences through the Integer Queue's operand
  wakeup, so instructions issue only once their physical sources are ready.
- Implements precise exceptions: on a fault the pipeline is flushed, the rename state is
  rolled back through the Active List in reverse order, and control transfers to the
  handler with the architectural state exactly as it was at the faulting instruction.
- Validated cycle by cycle against reference traces across the full test set.

```bash
./build.sh
./run.sh <input.json> <output.json>
./testall.sh              # run and diff the whole test set
```

C++17, no dependencies beyond a bundled nlohmann/json.

---

EPFL CS-470 Advanced Computer Architecture. The test harness and JSON I/O format were
provided by the course; the simulator is mine.
