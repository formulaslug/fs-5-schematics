# Fuse Board

## Purpose
The fuse board distributes LV 12 V power to the main vehicle subsystems.

It has nine protected branches:

- PEB
- TBB
- Telemetry
- Shutdown
- Inverter
- VCU
- Fan A
- Fan B
- Fan C

Each branch uses a TPS25983 eFuse for protection and current monitoring.

## Enable Control
Each eFuse has a 10 kOhm pullup to 3.3 V, so all branches are ON by default when the MCU boots.

The STM32G441KBT6 controls each eFuse with an open-drain GPIO:

- GPIO released = eFuse ON
- GPIO pulled low = eFuse OFF

## Current Sensing
Each eFuse has its own `IMON` resistor and filter.

Eight current signals go through the TMUX1308QPWRQ1 8:1 analog mux. The MCU uses three GPIOs to choose which branch to read.

The shutdown current signal goes directly to a second ADC input.

This lets us measure all nine branch currents using two ADC inputs.

The ADC inputs have RC filters and voltage clamps to reduce noise and protect the MCU.

## CAN
The MCU will send fuse-board data over CAN.

We still need to confirm the CAN connector, harness pinout, and termination with the electrical team.

## Status LEDs
We are planning to use an external WS2813B-compatible RGB LED strip instead of individual LEDs on the board.

We still need to confirm the connector, LED colors/states, and firmware timing.

## References
- TPS25983 datasheet
- TMUX1308QPWRQ1 datasheet
- STM32G441KBT6 datasheet
- Current-limit and timing calculations: to be added