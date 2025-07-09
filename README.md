# carrier_tracker

## Overview

This project is a software prototype for tracking a pure CW carrier from a satellite, including Doppler effects, using GNU Radio.  

The goal is to:

- Predict Doppler shift from TLE data for a satellite transmitting a ~10 GHz CW beacon.
- Build a signal processing chain that can track the carrier in real-time.
- Eventually use this flow with real hardware (SDR), but initially test entirely in software.

---

## Approach

We're simulating Doppler by creating a frequency-swept test signal in GNU Radio.  

### Test Signal Generation

- **Sawtooth Signal Source**
  - Generates a slow ramp waveform (e.g. 1/60 Hz) to simulate changing Doppler.
- **Add Const Block**
  - Shifts the sawtooth range from negative to positive frequencies.
- **Multiply Const Block**
  - Scales frequency to radians per sample for the VCO.
- **VCO (Voltage Controlled Oscillator)**
  - Converts the ramp into a drifting cosine carrier, simulating the CW beacon with Doppler.

---

## Receiver Tracking Methods

Two main ideas for tracking:

1. **FFT Peak Tracking**
   - Use an FFT to detect the carrier frequency peak and track changes frame by frame.
2. **PLL Tracking**
   - Feed the signal into a PLL to lock onto the carrier and extract frequency offset.

Our initial testing focuses on generating the Doppler-swept carrier and verifying it in a frequency sink.

---

## Blocks Being Tested

- `Signal Source` (Sawtooth)
- `Add Const`
- `Multiply Const`
- `VCO`
- `Complex to Real`
- `QT GUI Frequency Sink`
- (Planned) `PLL Carrier Tracking` or custom loop for frequency estimation

---

## Next Steps

- Confirm smooth frequency sweep of simulated carrier.
- Implement PLL tracking block for fine Doppler correction.
- Integrate TLE-based Doppler predictions for initial frequency estimates.

---

## Why This Matters

Accurately tracking a drifting CW carrier is critical for high-frequency satellite communication (e.g. 10 GHz) where Doppler shifts can reach tens of kHz in LEO passes.