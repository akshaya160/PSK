# PSK
# Aim
Write a simple Python program for the modulation and demodulation of PSK and QPSK.
# Tools required
Google colab Python
# Program
# PSK
```
import numpy as np
import matplotlib.pyplot as plt
fs = 1000         
Tb = 1              
fc = 5              
data = [1,0,1,1,0,1]

t = np.arange(0, Tb, 1/fs)
message = np.repeat(data, len(t))
time = np.arange(len(message)) / fs
carrier = np.sin(2*np.pi*fc*time)
# 1 -> +1 , 0 -> -1
polar_data = 2*message - 1
psk = polar_data * carrier
demod = psk * carrier
reconstructed = (demod > 0).astype(int)
plt.figure(figsize=(12,8))

plt.subplot(4,1,1)
plt.plot(time, message)
plt.title("Message Signal")
plt.ylim(-0.5,1.5)
plt.grid()

plt.subplot(4,1,2)
plt.plot(time, carrier)
plt.title("Carrier Signal")
plt.grid()

plt.subplot(4,1,3)
plt.plot(time, psk)
plt.title("PSK Modulated Signal")
plt.grid()

plt.subplot(4,1,4)
plt.plot(time, reconstructed)
plt.title("Demodulated Signal")
plt.ylim(-0.5,1.5)
plt.grid()

plt.tight_layout()
plt.show()

# QPSK
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
fs = 1000                # Sampling frequency
f_carrier = 50           # Carrier frequency
bit_rate = 10            # Data rate (bits per second)
T = 1                    # Total time duration in seconds
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
plt.ylabel('Amplitude')
plt.grid(True)
plt.subplot(4, 1, 2)
plt.plot(t, carrier, label='Carrier Signal', color='g')
plt.title('Carrier Signal')
plt.ylabel('Amplitude')
plt.grid(True)
plt.subplot(4, 1, 3)
plt.plot(t, psk_signal, label='PSK Modulated Signal', color='r')
plt.title('PSK Modulated Signal')
plt.ylabel('Amplitude')
plt.grid(True)
plt.subplot(4, 1, 4)
plt.step(np.arange(len(decoded_bits)), decoded_bits, label='Decoded Bits', color='r', marker='x')
plt.title('Decoded Bits')
plt.xlabel('Time')
plt.ylabel('Bit Value')
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()

```
# Output Waveform
# PSK

![WhatsApp Image 2025-10-06 at 14 09 22_c5e3e923](https://github.com/user-attachments/assets/c8745dcd-aef8-44ee-85ad-57e1b5db66ff)

# QPSK

![WhatsApp Image 2025-10-06 at 14 11 41_2c988368](https://github.com/user-attachments/assets/bb8f51d0-a234-4d41-b90a-1ff27ab26d29)

# Results
Thus the simple Python program for the modulation and demodulation of PSK and QPSK is done and verified.
