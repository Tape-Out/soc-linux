# soc-linux

Reference SoC on the way to Linux: a supervisor-capable core, first level caches and a PLIC.

![maturity](https://img.shields.io/badge/maturity-planned-lightgrey) ![license](https://img.shields.io/badge/license-MIT%20OR%20Apache--2.0%20OR%20MulanPSL--2.0-blue)

Part of the [Tape-Out](https://github.com/Tape-Out) IP library: Bluespec IP over the
bus-neutral contracts in [`hwcore`](https://github.com/Tape-Out/hwcore), assembled by
[`xirang`](https://github.com/Tape-Out/xirang). Maturity runs `planned` -> `simulated` ->
`fpga-proven` -> `asic-ready` -> `silicon-proven`.

## Status

Assembled and tested end to end in CI; the badge stays at `planned` while the assembler is being reworked. It does not boot Linux yet.

| Part | Repository | Configuration |
|:--:|:--:|:--:|
| core | [`hart`](https://github.com/Tape-Out/hart) | RV32IM with supervisor mode; the Sv32 MMU is off |
| caches | [`cache`](https://github.com/Tape-Out/cache) ×2 | instruction and data, 8 lines of 4 words, direct mapped, write through |
| memory | [`sram`](https://github.com/Tape-Out/sram) | 1024 words (4 KiB) at `0x8000_0000` |
| interrupts | [`aclint`](https://github.com/Tape-Out/aclint) · [`plic`](https://github.com/Tape-Out/plic) | one hart with supervisor software interrupts · 8 sources, 2 contexts |
| console | [`uart`](https://github.com/Tape-Out/uart) | at `0x1000_1000` |

Fetch and load/store each go through their own cache, and the caches' lower ports share the switch. All four interrupt lines of the core are wired: machine software and timer from the CLINT, external from the PLIC, and the supervisor software edge. What Linux still needs: the MMU turned on, far more memory than 4 KiB, and the A extension, which `hart` does not have.

## License

任选其一：

- [MIT](LICENSE-MIT)
- [Apache 2.0](LICENSE-APACHE)
- [木兰宽松许可证 第2版](LICENSE-MULAN)

`SPDX-License-Identifier: MIT OR Apache-2.0 OR MulanPSL-2.0`

除非另行说明，你提交的贡献按上述三者同时授权，不附加其他条件。
