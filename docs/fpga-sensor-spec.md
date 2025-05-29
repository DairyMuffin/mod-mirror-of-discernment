# FPGA Sensor Integration Specification — Mirror of Discernment (MoD)

## 1. Overview
The FPGA module captures biometric and environmental resonance signals (e.g., heart rate, brainwaves, ambient EM) and streams data via Bluetooth/Wi-Fi to the MoD gateway.

---

## 2. Hardware Requirements
- **FPGA Board:** e.g. Lattice iCE40 or Xilinx Artix-7  
- **Sensors:**  
  - PPG heart-rate sensor (e.g., MAX30102)  
  - EEG module (e.g., OpenBCI Cyton)  
  - IMU for motion (e.g., MPU-9250)  
  - Optional: EM field probe  

- **Comm Module:**  
  - Bluetooth Low Energy (BLE) PHY (e.g., Nordic nRF52840)  
  - Or Wi-Fi module (e.g., ESP32)

---

## 3. FPGA Logic Blocks
1. **Sensor Interfaces (I²C/SPI):**  
   - PPG & IMU via I²C  
   - EEG ADC via SPI  

2. **Preprocessing Pipeline:**  
   - FIR filter block for PPG/EEG signals  
   - Windowed FFT core for frequency analysis  
   - Peak detection module for heart-rate BPM  

3. **Phase-Locked Loop (PLL) Blocks:**  
   - Nested PLL for heartbeat sync  
   - Secondary PLL for EEG alpha/beta bands  
   - Tertiary PLL referencing ambient field probe  

4. **Data Aggregator & Serializer:**  
   - Combine filtered streams into packet (JSON or binary)  
   - Timestamp & phase metadata  

5. **Comm Interface:**  
   - UART to BLE/Wi-Fi SoC  
   - SPI/UART handshake protocol  

---

## 4. Firmware & Drivers
- **FPGA Bitstream:**  
  - Generated via vendor toolchain (IceStorm for Lattice, Vivado for Xilinx)  
  - Core constraints in `fpga/constraints.xdc`  

- **SoC Firmware (BLE):**  
  - Nordic SDK or ESP-IDF application  
  - Advertises sensor data with custom GATT service  

---

## 5. Data Format
```json
{
  "timestamp": 1685400000,
  "bpm": 72,
  "eeg": {
    "alpha": 8.5,
    "beta": 15.3
  },
  "imu": {
    "accel": [0.01, -0.02, 0.98],
    "gyro": [0.1, 0.0, -0.05]
  },
  "phase": {
    "beat": 4,
    "click": true
  }
}

