# EE430 Digital Signal Processing Term Project

## Part 2: Frequency-Hopping Spread Spectrum (FHSS) Implementation

### Introduction
Frequency-Hopping Spread Spectrum (FHSS) is a secure and robust communication method that spreads signals over a wide bandwidth by rapidly switching carrier frequencies in a pseudorandom sequence. This project focuses on implementing an FHSS transmitter and receiver system using MATLAB. The transmitter encodes digital data with M-Frequency Shift Keying (M-FSK), generates frequency-hopping signals, and visualizes them with spectrograms. The receiver decodes these signals while addressing challenges like noise and synchronization. By exploring FHSS's practical applications, this work demonstrates its resilience to interference and suitability for secure communication.

---

### Noise Reduction
1. **Short-Time Fourier Transform (STFT)**: The STFT of the received signal is calculated.
2. **Dominant Frequency Detection**: The dominant frequencies at each time index are identified by maximizing power across frequency indices.
3. **Median Filtering**: A Median Filter is applied to refine the dominant frequencies, preserving edges while suppressing impulsive noise.

---

### Keeping Track of the Hopping Frequency
- For each time interval (Th), a frequency (fk) is pseudorandomly selected from a predefined table.
- The frequency changes according to these conditions:
  - 500 Hz for Category 1
  - 1000 Hz for Category 2
  - 1500 Hz for Category 3
- The generated signal is normalized and saved as an audio file, including a starting and ending pilot signal to facilitate synchronization.

On the receiver side:
- Signals are segmented by leveraging the piecewise constant nature of dominant frequencies.
- Median filtering is applied within each segment to refine frequency values.
- Each segment carries information for one hopping period (Th).
- The nearest fk value is identified for each dominant frequency without considering the previous hopping frequency.

---

### Encoding Process
1. **Input Filtering**: Converts input string to lowercase and retains only alphabetic characters (letters 'a' through 'z').
2. **Alphabet Mapping**: Maps each letter to its position in the alphabet (e.g., 'a' -> 1, 'b' -> 2, ..., 'z' -> 26).
3. **Binary Conversion**: Represents indices as fixed-length 6-bit binary strings.
4. **Chunking**: Splits binary strings into n-bit chunks based on the modulation scheme.
5. **Symbol Mapping**:
   - 2-FSK: Maps bits to -1 or +1
   - 4-FSK: Maps bits to -2, -1, +1, or +2
   - 8-FSK: Maps bits to values ranging from -4 to +4

The numeric symbols are stored as a column vector compatible with the specified n-FSK scheme.

---

### Demodulation Process
1. **Frequency Offset Calculation**: For each segment, the hopping frequency is subtracted from the corresponding segment frequency (∆fmk).
2. **Binary Conversion**:
   - Example for 4-FSK decoding:
     ```matlab
     if m_k(i) < -1
         binaryChunks(i) = "00";
     elseif m_k(i) == -1
         binaryChunks(i) = "01";
     elseif m_k(i) == +1
         binaryChunks(i) = "10";
     elseif m_k(i) > +1
         binaryChunks(i) = "11";
     end
     ```
3. **Binary String Processing**:
   - Concatenates binary chunks.
   - Pads the binary string to a length divisible by 6.
   - Splits the padded string into groups of 6 bits.
   - Converts groups to decimal values and maps them to alphabet letters (1 -> 'a', 2 -> 'b', ..., 26 -> 'z').

---


This project demonstrates a comprehensive approach to implementing an FHSS communication system with robust techniques for noise reduction, frequency hopping, encoding, and demodulation.

