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

## eFuse calculations/configuration
To match the behavior of the previous blade fuses, we chose to use the TPS25983 eFuses, O-variant so that the fuse trips after only a short blanking limit as the current limit is reached, and we added some margin above the 10A/5A to ensure that the fuse allows current delivery continuously at 10A/5A instead of cutting power. 

We chose the O-variant so it would trip instead of current limiting, which the L-variant does:
"The TPS25983xL (Current Limiter) variants respond to output overcurrent conditions by actively regulating the
current to a set limit after a user adjustable fault blanking interval." (7.3.3.3)

PEB, Telemetry, TBB (example vals from 8.3.1.3 External Component Settings):
Blade Current ATM: 5A
eFuse Current Limit Set: 7A (7.0624A)
R_ILIM: 210 Ohm
R_IMON: 1.91 kOhm
C_ITIMER: 15nF
C_dVdt: 3.3nF

V_IMON_MAX = 3.28V (@ 7.0624A)
SR_ON = 1.15 V/ms @ 12V (Table 6.7)
t_D,ON = 3.83 ms @ 12V (Table 6.7)
t_ITIMER = 7ms

Shutdown, Inverter, VCU:
Blade Current ATM: 10A
eFuse Current Limit Set: 13A (13.0304A)
R_ILIM: 113 Ohm
R_IMON: 1.02 kOhm
C_ITIMER: 15nF
C_dVdt: 3.3nF

V_IMON_MAX = 3.23V (@ 13.0304A)
SR_ON = 1.15 V/ms @ 12V (Table 6.7)
t_D,ON = 3.83 ms @ 12V (Table 6.7)
t_ITIMER = 7ms

FANA, FANB, FANC:
Blade Current ATM: 10A
eFuse Current Limit Set: 13A (13.0304A)
R_ILIM: 113 Ohm
R_IMON: 1.02 kOhm
C_ITIMER: 15nF
C_dVdt: 3.3nF

V_IMON_MAX = 3.23V (@ 13.0304A)
SR_ON = 1.15 V/ms @ 12V (Table 6.7)
t_D,ON = 3.83 ms @ 12V (Table 6.7)
t_ITIMER = 7ms

Eqs (rounded down to closest standard val for passives):
R_ILIM(Ohm) = 1460/(I_LIM(A)-0.11) (7.3.3.2)
I_LIM(A) = (1460/R_ILIM(Ohm))+0.11

R_IMON(Ohm) = V_IMON_MAX(V)/(I_OUT_MAX(A) x 243 x 10^-6) (8.2.2.4)

t_ITIMER(ms) = (C_ITIMER(nF) x dV_ITIMER(V))/I_ITIMER(µA) (7.3.3.2)
must satisfy:
C_ITIMER < t_GHI/53000
t_GHI = t_D,ON + C_dVdt x ((V_IN + 3.6V)/I_dvdt)

Constants (Table 6.5):
dV_ITIMER = 0.98 V
I_ITIMER = 2.1 μA
I_dVdt = 4.76 μA
ILIM Tolerance ~-13%/+10%

How 5A->7A and 10A->13A were chosen:
Minimum trip current should be above blade fuse's currents
Min = ILIM*0.87
7A*0.87 = 6.09A > 5A
13A*0.87 = 11.31A > 10A

Sheet for making sure there is no glitchy startup: https://drive.google.com/file/d/1huV-2nOIGhpAAAtI-BUvq7ih7KmQudSw/view?usp=sharing

## References
- TPS25983 datasheet
- TMUX1308QPWRQ1 datasheet
- STM32G441KBT6 datasheet
- Current-limit and timing calculations: to be added