# 8-bit CMOS Arithmetic Unit

![Logisim-evolution](https://img.shields.io/badge/Logisim--evolution-Digital_Logic-267A38?style=flat-square)
![Magic](https://img.shields.io/badge/Magic-VLSI_Layout-7D4698?style=flat-square)
![IRSIM](https://img.shields.io/badge/IRSIM-Switch_Level_Simulation-2366A5?style=flat-square)
![CMOS](https://img.shields.io/badge/Technology-SCMOS-455A64?style=flat-square)

**Talal Aloushan · December 2023 · CEN415 - Introduction to VLSI Design**  

A VLSI course project implementing an arithmetic unit with two 8-bit inputs, a 2-bit selector, and a 16-bit output. The work spans digital logic design in Logisim, hierarchical CMOS layout in Magic, and switch-level simulation in IRSIM. Four arithmetic paths combine multiplication(parallel CSA & CPA), subtraction, and addition with constant multiplication, using full adders, CMOS gates, and multiplexers.



## Problem

Implement a system that performs the operations in the table:

<table>
<thead>
<tr><th colspan="2">Operation code</th><th rowspan="2">Operation (Y)</th><th rowspan="2">Description</th></tr>
<tr><th>s1</th><th>s0</th></tr>
</thead>
<tbody>
<tr><td>0</td><td>0</td><td>A * B</td><td>Multiply A by B using parallel CSA and CPA</td></tr>
<tr><td>0</td><td>1</td><td>A * B</td><td>Multiply A by B using shift and add.</td></tr>
<tr><td>1</td><td>0</td><td>A - B</td><td>Simple subtraction</td></tr>
<tr><td>1</td><td>1</td><td>5 * (A+B)</td><td>Multiply the addition result by 5</td></tr>
</tbody>
</table>

Inputs: `A[7:0]`, `B[7:0]`, `S[1:0]`, VDD and GND. Outputs: `Y[15:0]` and a possible carry-out. Device dimensions: nMOS `W = 4λ, L = 2λ`; pMOS `W = 8λ, L = 2λ`.

## Results



| Operation | Example | Expected result |
|---|---|---:|
| Parallel CSA/CPA multiplication | `7 × 7` | 49 |
| Shift-and-add multiplication | `7 × 7` | 49 |
| Subtraction | `7 − 7` | 0 |
| Addition followed by multiplication by five | `5 × (3 + 5)` | 40 |

<details open>
<summary><strong>Integrated design and simulation</strong></summary>

| Logisim | Magic layout |
|:---:|:---:|
| <img src="assets/logisim.png" alt="Original integrated Logisim circuit" width="460"> | <img src="assets/layout.png" alt="Original final Magic layout" width="460"> |

![Final-design IRSIM waveform](assets/verification.png)


</details>

## Approach

Arithmetic units feed a bank of 4:1 output multiplexers. Full adders and basic CMOS gates form the reusable building blocks.

<details open>
<summary><strong>00 — Parallel CSA and CPA multiplication</strong></summary>

AND gates form partial products; carry-save stages reduce them before a final carry-propagate addition.

| Logisim | Magic layout |
|:---:|:---:|
| <img src="assets/logisim-csa-cpa.png" alt="Original CSA and CPA Logisim circuit" width="460"> | <img src="assets/layout-multiplier.png" alt="CSA and CPA multiplier layout from the report" width="460"> |

</details>

<details open>
<summary><strong>01 — Shift-and-add multiplication</strong></summary>

Shifted partial products are accumulated through the combinational adder network.

| Logisim | Magic layout |
|:---:|:---:|
| <img src="assets/logisim-shift-add.png" alt="Original shift-and-add Logisim circuit" width="460"> | <img src="assets/layout-multiplier.png" alt="Shift-and-add multiplier layout shown in the report" width="460"> |



</details>

<details open>
<summary><strong>10 — Subtraction</strong></summary>

The add/subtract unit uses conditional inversion and carry-in to implement `A + (~B) + 1`.

| Logisim | Magic layout |
|:---:|:---:|
| <img src="assets/logisim-subtractor.png" alt="Original add-subtract Logisim circuit" width="460"> | <img src="assets/layout-subtractor.png" alt="Original eight-bit subtractor Magic layout" width="460"> |

</details>

<details open>
<summary><strong>11 — Addition followed by multiplication by five</strong></summary>

A ripple-carry adder forms `T = A + B`; constant multiplication follows `5T = (T << 2) + T`.

| Logisim | Magic layout |
|:---:|:---:|
| <img src="assets/logisim-add-times-five.png" alt="Original addition and constant-multiplication Logisim circuit" width="460"> | <img src="assets/layout-add-times-five.png" alt="Original addition and multiplication-by-five Magic layout" width="460"> |



</details>

<details open>
<summary><strong>CMOS gate layouts</strong></summary>

| NOT | NAND | NOR |
|:---:|:---:|:---:|
| <img src="gates_layout/not.JPG" alt="Original inverter layout" width="260"> | <img src="gates_layout/nand.JPG" alt="Original NAND layout" width="260"> | <img src="gates_layout/nor.JPG" alt="Original NOR layout" width="260"> |
| **AND** | **OR** | **XOR** |
| <img src="gates_layout/and.JPG" alt="Original AND layout" width="260"> | <img src="gates_layout/or.JPG" alt="Original OR layout" width="260"> | <img src="gates_layout/xor.JPG" alt="Original XOR layout" width="260"> |
| **4:1 MUX** | | |
| <img src="assets/layout-mux.png" alt="Original 4-to-1 multiplexer Magic layout" width="260"> | | |

</details>

## Project files

| Path | Contents |
|---|---|
| [`Logisim/`](Logisim/) | Integrated `VLSIPROJ.circ`, CSA/CPA library, and separate `csa.circ` schematic |
| [`mainp/`](mainp/) | Magic layouts, extraction files, IRSIM netlists and test commands; top level: `Final.mag` |
| [`gates_layout/`](gates_layout/) | gate layout images |
| [`docs/VLSI451Report.pdf`](docs/VLSI451Report.pdf) · [Word source](docs/VLSI451Report.docx) | Project report and results |
| [`docs/assignment.pdf`](docs/assignment.pdf) | Original problem statement |
| [`docs/Magic-irsim-tutorial.pdf`](docs/Magic-irsim-tutorial.pdf) | Magic and IRSIM tutorial |
| [`docs/Preparing-Project-Environment.pdf`](docs/Preparing-Project-Environment.pdf) | Project environment guide |
| [`assets/`](assets/) | Individual README figures |
