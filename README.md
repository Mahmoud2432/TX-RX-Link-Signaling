# TX-RX-Link-Signaling
An Adaptive 12 Gbps High-Speed Serial Transceiver.

- Project Overview
This repository contains the design and implementation of a high-speed 12 Gbps Serializer/Deserializer (SerDes) link. The system addresses signal integrity challenges such as Inter-Symbol Interference (ISI) and channel loss through an adaptive equalization architecture. It features a robust Transmitter (TX) with feed-forward equalization and a Receiver (RX) equipped with a CTLE and DFE to open the data eye at 12 Gbps.

The project was designed by a team of 6 undergraduate students  and simulates a complete mixed-signal communication link.

# System Architecture

**1- Transmitter (TX)** :
The Transmitter converts 8-bit parallel low-speed data into a 12 Gbps serial stream with pre-distortion to combat channel loss. 

- 8-bit Serializer: Uses a tree-structure architecture (MUX 8:4 → 4:2 → 2:1) to serialize data. The final stage operates at full rate (12 Gbps) using CML logic for high speed.

- Voltage Mode (VM) Driver: A differential driver based on SST (Source-Series Terminated) slices. It uses binary-weighted slices (1x to 32x) to control impedance and termination (50Ω target).

- Feed-Forward Equalizer (FFE): A 1-tap FFE (Pre-cursor and Main-cursor) implementation that pre-distorts the signal to cancel ISI. Tap weights are programmable to optimize the eye opening.

- Verilog-A Controller: Used for swing control, calibration of the termination resistance, and FFE tap weight adaptation.


**2- Channel** :
The system is tested over a differential channel with approximately -16.8 dB loss at 6 GHz (Nyquist frequency).


**3- Receiver (RX)** :
The Receiver recovers the 12 Gbps serial data, equalizes channel loss, and deserializes the output.

- Variable Gain Amplifier (VGA): The first stage of the RX. It provides adjustable gain to normalize signal amplitude for the subsequent equalizer stages, ensuring optimal processing range.

- Continuous-Time Linear Equalizer (CTLE): An analog equalizer that boosts high-frequency components to compensate for channel loss. It provides ~8 dB of peaking at the Nyquist frequency.

- Decision Feedback Equalizer (DFE): A 1-tap DFE that removes post-cursor ISI using a decision-feedback loop. It uses a Sign-Sign LMS algorithm for adaptive weight updates.

- Comparators :
High-speed comparators convert the equalized analog signal to digital bits.

- Deserializer :
The digital bits from the comparator is deserialized back into an 8-bit parallel bus.

