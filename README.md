# Design of Differential-mode (DM) Active EMI Filter for LLC Converters

This project focuses on the design and implementation of an Active Electromagnetic Interference (EMI) Filter specifically targeting Differential-Mode (DM) noise in LLC converters. The project was authored by Suhani Biswal.

Differential-mode noise originates from the switching node, generating high di/dt that travels across the circuit and into power lines. In DM noise, the outgoing and return currents flow in opposite directions, with the noise current flowing on the same path as the power supply current.

## Active EMI Filter Architecture

Unlike passive filters, an Active EMI Filter (AEF) circuit senses the residual disturbance (voltage or current) and injects an opposing signal to directly negate the noise via destructive interference.

*   **Topology:** The chosen topology is Voltage-Sensing Current-Injection (VSCI).
*   **Sensing and Injection:** The circuit utilizes capacitive sensing and capacitive injection.
*   **Benefits of VSCI:** This selection eliminates the need for bulky high-frequency current transformers. It results in a smaller size and lower cost. Furthermore, it is highly suitable for high-frequency EMI suppression and integrates easily with existing X-capacitor-based DM filters.
*   **Capacitive Amplification:** The core principle uses a feedback loop (the Miller effect) to increase the equivalent capacitance of the X-capacitor at high frequencies. This solves the sizing constraints of the X-capacitor and allows for a smaller DM inductor.

## Passive Filter Design

A $\pi$-LC filter was selected in conjunction with the AEF over a simple LC filter to protect the active components and improve attenuation.

*   **Op-Amp Protection:** Op-amp circuitry easily saturates and is highly sensitive to high current peaks. The $\pi$-LC filter provides isolation from high-frequency dv/dt spikes originating from the EUT (Equipment Under Test) side.
*   **Regulated Impedance:** The LC pair on the LISN (Line Impedance Stabilization Network) side localizes the cancellation current and protects the op-amp from source-side EMI creeping in.
*   **Superior Attenuation:** The $\pi$-LC filter delivers a -60dB/decade attenuation compared to the -40dB/decade of a simple LC filter. The cut-off frequency is set to 118kHz.
*   **Resonance Damping:** An RC snubber circuit is included to dampen LC resonance. Without the snubber, a large low-frequency component distorts the op-amp output.

## Control Loop and Stability

The control network includes a compensation network configured to maintain stability in the operational amplifier (which features a 120MHz Gain Bandwidth Product).

*   **Poles and Zeros:** The system features a low-frequency pole at 33kHz to set a baseline gain and a stabilizing zero at 225kHz to allow the op-amp to fight harder in the 500kHz to 1MHz zone.
*   **Phase Margin:** The system achieves a phase margin of 105 degrees at a gain crossover frequency of 13MHz. Since the minimum phase margin required is 45 degrees, this ensures a completely stable system.

## Results and Component Reduction

The target EMI limit for the design is CISPR 32 Class A, evaluated using quasi-peak (QP) and average (AVG) detectors.

### LLC Converter Results
*   **Without Filter:** Noise levels reached a high of 136.32 dBuV.
*   **Passives Only:** A purely passive $\pi$-LC filter with a snubber lowered the peak to 74.40 dBuV, which still fails regulatory standards.
*   **Passives + AEF:** Activating the AEF reduced the highest peak to 59.33 dBuV (at 2MHz), safely passing both the QP and AVG limits.
*   **Component Footprint Reduction:** To achieve passing attenuation with only passive filters, a 2.5uH inductor and a 1uF X-capacitor are required. Integrating the AEF enabled a 50% decrease in the X-capacitor size (down to 500nF) and a 30% decrease in the inductor size (down to 1.8uH).

### Sentry Schematic Results
*   **Without Filter:** Baseline noise reached 85.38 dBuV.
*   **Passives + AEF:** Successfully reduced the peak noise to 56.12 dBuV (at 1MHz), safely below CISPR 32 Class A limits.
*   **Component Footprint Reduction:** Yielded a 30% reduction in both the X-capacitor (from 800nF down to 500nF) and inductor (from 800nH down to 500nH) sizes when compared to a passive-only design offering the same attenuation.

## Common Mode to Differential Mode (C2D) Conversion

In addition to the primary DM noise filter, the project highlights a method to reduce Common Mode (CM) noise by actively converting it to DM noise. This is achieved by actively forcing the noise from the ground into the neutral line using an active C2D circuit.
