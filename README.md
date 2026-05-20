# Ex: 5- PSK and QPSK
## AIM:
Write a simple Python program for the modulation and demodulation of PSK and QPSK.
## TOOLS REQUIRED:
Google Colab
## PROGRAM:
#### PSK:
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Low-pass filter
def lpF(x, fc, fs):
    b, a = butter(4, fc / (0.5 * fs), btype='low')
    return lfilter(b, a, x)

# Parameters
fs = 1000          # Sampling frequency
fc = 50            # Carrier frequency
br = 10            # Bit rate
bc = 8             # Number of bits
T = bc / br        # Total time duration

# Time axis
t = np.arange(0, T, 1/fs)

# Samples per bit
bd = int(fs / br)

# Message bits
bits = np.random.randint(0, 2, bc)

# Convert bits to NRZ signal
msg = np.repeat(bits, bd)

# Match time vector length
t = t[:len(msg)]

# Carrier signal
carrier = np.sin(2 * np.pi * fc * t)

# BPSK Modulation
bpsk = np.sin(2 * np.pi * fc * t + np.pi * msg)

# Coherent Demodulation
product = bpsk * carrier
demod = lpF(product, fc, fs)

# Bit decoding
sample_points = np.arange(bd//2, len(demod), bd)
decoded = (demod[sample_points] < 0).astype(int)

# Plotting
plt.figure(figsize=(10, 9))

# Name and Register Number
plt.suptitle("NAME : TAMILSELVAN\nREG NO : 212224060275",
             fontsize=12, fontweight='bold')

plt.subplot(4, 1, 1)
plt.plot(t, msg)
plt.title("Message Signal")
plt.grid(True)

plt.subplot(4, 1, 2)
plt.plot(t, carrier)
plt.title("Carrier Signal")
plt.grid(True)

plt.subplot(4, 1, 3)
plt.plot(t, bpsk)
plt.title("BPSK Modulated Signal")
plt.grid(True)

plt.subplot(4, 1, 4)
plt.step(range(len(decoded)), decoded, where='mid')
plt.title("Decoded Bits")
plt.ylim(-0.2, 1.2)
plt.grid(True)

plt.tight_layout(rect=[0,0,1,0.93])
plt.show()

print("Original Bits :", bits)
print("Decoded Bits  :", decoded)
```
#### QPSK:
```
import numpy as np
import matplotlib.pyplot as plt

# Parameters
fs = 1000          # Sampling frequency
fc = 10            # Carrier frequency
T = 1              # Total duration

# Time axis
t = np.arange(0, T, 1/fs)

# Input bit pairs
bits = np.array([1, 0, 1, 1, 1, 1, 1, 0])   # 10 11 11 10
symbols = bits.reshape(-1, 2)

# Samples per symbol
symbol_samples = len(t) // len(symbols)

# QPSK Modulation (I-Q method)
qpsk = np.zeros(len(t))

for i, pair in enumerate(symbols):
    I = 1 if pair[0] == 1 else -1
    Q = 1 if pair[1] == 1 else -1

    ts = t[i*symbol_samples:(i+1)*symbol_samples]

    qpsk[i*symbol_samples:(i+1)*symbol_samples] = (
        I * np.cos(2*np.pi*fc*ts) +
        Q * np.sin(2*np.pi*fc*ts)
    )

# Demodulation
decoded = []

for i in range(len(symbols)):
    ts = t[i*symbol_samples:(i+1)*symbol_samples]
    segment = qpsk[i*symbol_samples:(i+1)*symbol_samples]

    I_demod = np.sum(segment * np.cos(2*np.pi*fc*ts))
    Q_demod = np.sum(segment * np.sin(2*np.pi*fc*ts))

    decoded.append(1 if I_demod > 0 else 0)
    decoded.append(1 if Q_demod > 0 else 0)

# Plot
plt.figure(figsize=(10,8))

plt.suptitle("NAME : TAMILSELVAN\nREG NO : 212224060275",
             fontsize=12, fontweight='bold')

plt.subplot(3,1,1)
plt.step(range(len(bits)), bits, where='mid')
plt.title("Input Binary Data")
plt.ylim(-0.5, 1.5)
plt.grid(True)

plt.subplot(3,1,2)
plt.plot(t, qpsk)
plt.title("QPSK Modulated Signal")
plt.grid(True)

plt.subplot(3,1,3)
plt.step(range(len(decoded)), decoded, where='mid')
plt.title("Demodulated Output")
plt.ylim(-0.5, 1.5)
plt.grid(True)

plt.tight_layout(rect=[0,0,1,0.93])
plt.show()

print("Original Bits   :", bits)
print("Demodulated Bits:", decoded)
```
## OUTPUT WAVEFORM:
#### PSK:
<img width="810" height="732" alt="image" src="https://github.com/user-attachments/assets/82dbdf9e-b6d6-4ff7-82cf-6e1369b0baf0" />


#### QPSK:
<img width="821" height="658" alt="image" src="https://github.com/user-attachments/assets/d4a4134e-ae04-4428-b409-7dadf096185d" />



## RESULT:
The PSK and QPSK signals were successfully modulated and demodulated using Google Colab.
