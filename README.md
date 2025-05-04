# PSK Modulation and Demodulation
# Vignesh S
# 212223060300
# Aim
To implement a Python program for the modulation and demodulation of Phase Shift Keying (PSK) and visualize the output.
# Tools required
Python IDE with Numpy and Scipy
Google Colab (for running the program)
# Program
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Butterworth low-pass filter for demodulation
def butter_lowpass_filter(data, cutoff, fs, order=5):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff / nyquist
    b, a = butter(order, normal_cutoff, btype='low', analog=False)
    return lfilter(b, a, data)

# Parameters
fs = 1000  # Sampling frequency
f_carrier = 50  # Carrier frequency
bit_rate = 10  # Data rate
T = 1  # Total time duration
t = np.linspace(0, T, int(fs * T), endpoint=False)

# Message signal (binary data)
bits = np.random.randint(0, 2, bit_rate)
bit_duration = fs // bit_rate
message_signal = np.repeat(bits, bit_duration)

# PSK Modulation (0 -> 0 phase, 1 -> 180° phase shift)
carrier = np.sin(2 * np.pi * f_carrier * t)
psk_signal = np.sin(2 * np.pi * f_carrier * t + np.pi * message_signal)

# PSK Demodulation
demodulated = psk_signal * carrier
filtered_signal = butter_lowpass_filter(demodulated, f_carrier, fs)
decoded_bits = (filtered_signal[::bit_duration] < 0).astype(int)

# Plotting
plt.figure(figsize=(12, 8))
plt.subplot(4, 1, 1)
plt.plot(t, message_signal, label='Message Signal (Binary)', color='b')
plt.title('Message Signal')
plt.grid(True)

plt.subplot(4, 1, 2)
plt.plot(t, carrier, label='Carrier Signal', color='g')
plt.title('Carrier Signal')
plt.grid(True)

plt.subplot(4, 1, 3)
plt.plot(t, psk_signal, label='PSK Modulated Signal', color='r')
plt.title('PSK Modulated Signal')
plt.grid(True)

plt.subplot(4, 1, 4)
plt.step(np.arange(len(decoded_bits)), decoded_bits, label='Decoded Bits', color='r', marker='x')
plt.title('Decoded Bits')
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()
```
# Output Waveform
![image](https://github.com/user-attachments/assets/58ac1bcc-bd5a-409e-8165-7afd377b827a)
# Results
The experiment successfully demonstrated digital-analog conversion using PSK. The modulation and demodulation process was visually confirmed by comparing the message signal and the decoded bits after transmission through the PSK system.
# Hardware experiment output waveform.
![image](https://github.com/user-attachments/assets/556727ca-6cad-4e00-82c7-2ac207ed205b)
