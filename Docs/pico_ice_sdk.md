---
title: pico-ice-sdk
layout: default
nav_order: 3
has_children: true
---

# pico-ice-sdk

## Getting Started

This is a guide for how to build application running on the RP2040 microcontroller.

The [pico-ice-sdk](https://github.com/tinyvision-ai-inc/pico-ice-sdk/) provides an API for communicating with the pico-ice hardware, also allowing to use the Raspberry Pi [pico-sdk](https://github.com/raspberrypi/pico-sdk/) directly.

The pico-ice-sdk is organised as a normal pico-sdk project with `pico_ice` custom board.

A [template](https://github.com/tinyvision-ai-inc/pico-ice-sdk/blob/main/template/) shows how everything can be to get started.

## Troubleshooting

Feel free to join the [chat server](https://discord.gg/sb2kwc66) to ask for help.

### Using some RP2040 peripheral cause various bugs.

In order to power the FPGA, some peripherals and GPIO pins are in use by the pico-ice-sdk.
In case both the firmware and SDK use the same peripheral, it is possible to use another free peripheral instance, or if none left, disable the feature of the SDK
The `ice_init()` is responsible for setting-up all peripherals used by the SDK.
Instead, calling manually each `ice_init_<feature>()` of interest permits to select what to enable or not in the board, and therefore keeping some more peripherals for the user.

### Error: C++ compiler not installed on this system

The pico-sdk is written in C, but uses a single C++ file to enable C++ support in the SDK.
This means you need a working C++ cross compiler, often named `arm-none-eabi-g++`.

Even if this binary is present in your system, it might not be a full C++ installation.
If you do not need C++ and want to work around this bug, you can disable the C++ support
in the pico-sdk. From your project repo:

```shell
$ cd build
$ cmake .. # download the SDK if not yet done
$ sed -i '/new_delete.cpp/ d' _deps/pico-sdk-src/src/rp2_common/pico_standard_link/CMakeLists.txt
$ cmake .. # rebuild the Makefile with the fix
```