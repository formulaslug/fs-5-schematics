# Realtime Simulation interface for on-car hardware
The goal of this project is to act as an interface between the (python) full car simulation and on-car hardware (VCU, TBB, TPB) for tuning, testing, and development purposes.

The first step here should be an interace board which exposes lots of PWM and digital IO from an stm32G473 (either the nucleo, or a custom implementation of the MCU directly--up to you). Outputs should be on primarily HRTIM pins, with an RC filter in the tens of khz and then an op amp with adjustable gain and a really tall voltage rail (ideally +30v for use with BMS boards). ~35 "analog" outputs is ideal. digial IO should be populatable (like by soldering different components) as input or output and as direct, open drain, or level shifted (maybe an opto-emulator?). All the digital I/O ports should also have an indicator LED.

Then, all those signals should be routed to a really easy-to-use for prototyping connector like WAGO lever nuts or wiedmuller connectors (which are cheaper and higher density).

We can make this board (and future similar boards) mount to a DIN rail (which can also host relays and junction boards and stuff) and it should have a USB slave port (B or C please) to talk to a linux comptuer which will run the simulation. It should also expose all 3 FDCAN interfaces and have transievers for them.
