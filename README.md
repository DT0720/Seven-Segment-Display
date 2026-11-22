# 7-Segment Display Project 

## 1. Overview

This project builds a **4-input (A, B, C, D) to 7-segment (a–g) display driver** in two phases.

- **Phase 1, combinational:** the same truth table is implemented in several ways (SOP, POS, NAND-only, NOR-only, MUX, Decoder). The designs are compared by cost and the cheapest one is built in hardware.
- **Phase 2, sequential:** a clocked 4-bit counter built from flip-flops (J-K, T and D) steps through the group's display sequence. It drives the display through the same decoding logic.

The display sequence is a string of digits and the symbols `S`, `L`, `D` and `-`, as given in the handout. Each character corresponds to one 4-bit input code.

All circuits are in **Logisim** (`.circ` files).

---

## 2. Repository contents

| File | Phase | What it implements |
|---|---|---|
| `Project_CSE231_Sec-05.pdf` | — | Project handout (requirements and group display table) |
| `Sop_7_segment.circ` | 1 | **1st canonical form / SOP** using AND-OR with NOT gates |
| `Pos.circ` | 1 | **2nd canonical form / POS** using OR-AND with NOT gates |
| `Nand.circ` | 1 | **NAND-only** implementation |
| `Nor_gate.circ` | 1 | **NOR-only** implementation |
| `projectmux.circ` | 1 | **Multiplexer** implementation (7 muxes, one per segment) |
| `projectdecoder.circ` | 1 | **Decoder** implementation (4-to-16 decoder + OR gates) |
| `Project_JK.circ` | 2 | Sequential circuit using **J-K flip-flops** |
| `Project_T_flip_flop.circ` | 2 | Sequential circuit using **T flip-flops** |
| `Project_D_Flip_flop.circ` | 2 | Sequential circuit using **D flip-flops** |

---

## 3. How to run

1. Install **Logisim** (Logisim-evolution also works). Java is required.
2. Open any `.circ` file with `File → Open`.
3. **Combinational circuits** (Phase 1)
   - Select the **Poke tool** (hand icon).
   - Click the input pins `A B C D` to toggle them between 0 and 1.
   - The 7-segment display and the output pins `a`–`g` update immediately.
4. **Sequential circuits** (Phase 2)
   - Go to `Simulate → Ticks Enabled` (shortcut `Ctrl+K`) to start the clock.
   - Use `Simulate → Tick Frequency` to slow the clock if needed.
   - Press `Ctrl+T` to advance one tick manually.
   - The display advances through the group's sequence on each clock edge.
5. To reset, use `Simulate → Reset Simulation` (`Ctrl+R`).

---

## 4. Circuit details

### Phase 1: Combinational

All Phase 1 circuits share the same interface.

- **Inputs:** `A B C D` (A is the MSB)
- **Outputs:** segments `a b c d e f g`, wired to a 7-segment display

| Circuit | Gate count (from the `.circ` files) |
|---|---|
| SOP | 18 AND, 25 NOT, 7 OR |
| POS | 7 AND, 4 NOT, 20 OR |
| NAND-only | 48 NAND |
| NOR-only | 31 NOR |
| MUX | 7 MUX, 2 NOT |
| Decoder | 1 Decoder, 7 OR |

### Phase 2: Sequential

| Circuit | Flip-flops | Next-state logic (gate count) |
|---|---|---|
| J-K | 4 × J-K | 4 AND, 2 OR, 1 NOT |
| T | 4 × T | 3 AND, 3 OR, 1 NOT |
| D | 4 × D | 7 AND, 3 OR, 1 NOT, 1 XOR |

Each sequential circuit follows the same structure.

- A **Clock** (`CLK`) drives four flip-flops that hold the 4-bit state.
- A **Splitter** separates the state bits into `A B C D`.
- **Next-state logic** is derived from the excitation table of the chosen flip-flop.
- **Seven 8:1 multiplexers**, one per segment, drive the 7-segment display from the state bits.

---

## 5. Notes

- The circuits were checked in Logisim by toggling inputs or running the clock, and comparing the display with the expected sequence.
- Unused input codes are treated as don't-cares in the K-map simplification, where applicable.
- Segment labels follow the standard layout (a = top, b = top right, c = bottom right, d = bottom, e = bottom left, f = top left, g = middle).
