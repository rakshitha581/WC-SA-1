# WC-SA-1
**1.Write a MATLAB code to estimate call dropping probability in cellular networks.**

**Aim**:
    To estimate the call dropping probability in a cellular network using MATLAB simulation.
    
**Objective**:
    To simulate call attempts in a cellular network and calculate the probability of dropped calls under given network conditions
    
**Theory:**
    In cellular communication systems, call dropping occurs when an active call is terminated unexpectedly due to poor signal strength, interference, congestion, or mobility issues.         
Call Dropping Probability is defined as:
[
Call\ Dropping\ Probability = \frac{Number\ of\ Dropped\ Calls}{Total\ Number\ of\ Calls}
]
  In this simulation, random numbers are generated to represent call behavior. If the generated random value is below the specified threshold, the call is considered dropped.
  
**Code:**
```
import random

# Input parameters
total_calls = 1000        # Total number of call attempts
drop_threshold = 0.2      # 20% chance of call dropping

# Initialize dropped call counter
dropped_calls = 0

# Simulation of calls
for i in range(total_calls):
    
    # Generate random number between 0 and 1
    r = random.random()
    
    # Check whether call is dropped
    if r < drop_threshold:
        dropped_calls = dropped_calls + 1

# Calculate dropping probability
call_drop_probability = dropped_calls / total_calls

# Display results
print(f'Total Calls           = {total_calls}')
print(f'Dropped Calls         = {dropped_calls}')
print(f'Call Dropping Probability = {call_drop_probability:.4f}')
```
**Output:**
Total Calls           = 1000
Dropped Calls         = 193
Call Dropping Probability = 0.1930

**Result:**
  Thus, the MATLAB simulation for estimating call dropping probability in a cellular network was successfully implemented and analyzed.
  
  **2. Write a MATLAB code to evaluate outage probability in fading environments.
  
 **Aim**:**
    To evaluate the outage probability in a fading environment using MATLAB simulation.
    
**Objective:**
    To simulate a Rayleigh fading communication channel and calculate the outage probability based on signal-to-noise ratio (SNR).
    
**Theory:**
   In wireless communication systems, fading causes fluctuations in signal strength due to multipath propagation. When the received signal quality falls below a threshold value, communication failure occurs. This condition is called an outage.
 Outage Probability is defined as:

[
P_{out} = \frac{Number\ of\ Outage\ Events}{Total\ Number\ of\ Samples}
]

In Rayleigh fading environments, the channel gain varies randomly, affecting the instantaneous SNR.
An outage occurs when:
[
Instantaneous\ SNR < Threshold\ SNR
]

**Code:**
```
import numpy as np

# Input parameters
num_samples = 10000     # Number of signal samples
snr_avg_dB = 10         # Average SNR in dB
threshold_dB = 5        # Outage threshold in dB

# Convert dB to linear scale
snr_avg = 10**(snr_avg_dB/10)
threshold = 10**(threshold_dB/10)

# Generate Rayleigh fading channel
h = (np.random.randn(num_samples) + 1j*np.random.randn(num_samples))/np.sqrt(2)

# Instantaneous SNR
instant_snr = snr_avg * (np.abs(h)**2)

# Count outage events
outage_count = np.sum(instant_snr < threshold)

# Calculate outage probability
outage_probability = outage_count / num_samples

# Display results
print(f'Average SNR (dB)      = {snr_avg_dB} dB')
print(f'Threshold SNR (dB)    = {threshold_dB} dB')
print(f'Outage Probability    = {outage_probability:.4f}')
```
**Output:**
Average SNR (dB)      = 10 dB
Threshold SNR (dB)    = 5 dB
Outage Probability    = 0.2830

**Result:**
 Thus, the MATLAB simulation for evaluating outage probability in a Rayleigh fading environment was successfully implemented and analyzed.

**3.Write a MATLAB code to compare transmitter and receiver diversity performance.**

**Aim:**
   To compare the performance of transmitter diversity and receiver diversity techniques using MATLAB simulation.
   
**Objective:**
   To evaluate and compare the Bit Error Rate (BER) performance of transmitter diversity and receiver diversity systems under different Signal-to-Noise Ratio (SNR) conditions.
   
**Theory:**
    Diversity techniques are used in wireless communication systems to reduce fading effects and improve signal quality.
    
**Transmitter Diversity**
    In transmitter diversity, multiple transmitting antennas are used to send the same information signal. This improves transmission reliability.
**Receiver Diversity****
     In receiver diversity, multiple receiving antennas are used to receive the signal. Techniques such as Maximal Ratio Combining (MRC) improve reception quality.
The performance is measured using Bit Error Rate (BER):
[
BER = \frac{Number\ of\ Error\ Bits}{Total\ Number\ of\ Bits}
]

Lower BER indicates better communication performance.

**Code:**
```
import numpy as np
import matplotlib.pyplot as plt

# Parameters
N = 1000000;               # Number of bits (increased to capture more errors)
SNR_dB = np.arange(0, 21, 2);           # SNR range in dB

# Generate random bits
data = np.random.randint(0, 2, N);

# BPSK modulation
bpsk = 2*data - 1;

# Initialize BER arrays
BER_tx = np.zeros_like(SNR_dB);
BER_rx = np.zeros_like(SNR_dB);

for k in range(len(SNR_dB)):
    
    snr_linear = 10**(SNR_dB[k]/10);
    noise_std = np.sqrt(1/(2*snr_linear));
    
    ## Transmitter Diversity (2 Tx antennas)
    
    h1 = (np.random.randn(N)+1j*np.random.randn(N))/np.sqrt(2);
    h2 = (np.random.randn(N)+1j*np.random.randn(N))/np.sqrt(2);
    
    noise1 = noise_std*(np.random.randn(N)+1j*np.random.randn(N));
    
    # Alamouti-type transmission
    rx_tx = h1*bpsk + h2*bpsk + noise1;
    
    # Detection
    detected_tx = np.real(rx_tx) > 0;
    
    # BER calculation
    BER_tx[k] = np.sum(data != detected_tx)/N;
    
    ## Receiver Diversity (2 Rx antennas)
    
    h3 = (np.random.randn(N)+1j*np.random.randn(N))/np.sqrt(2);
    h4 = (np.random.randn(N)+1j*np.random.randn(N))/np.sqrt(2);
    
    noise2 = noise_std*(np.random.randn(N)+1j*np.random.randn(N));
    noise3 = noise_std*(np.random.randn(N)+1j*np.random.randn(N));
    
    # Signals at two receivers
    rx1 = h3*bpsk + noise2;
    rx2 = h4*bpsk + noise3;
    
    # Maximal Ratio Combining
    combined = np.conj(h3)*rx1 + np.conj(h4)*rx2;
    
    # Detection
    detected_rx = np.real(combined) > 0;
    
    # BER calculation
    BER_rx[k] = np.sum(data != detected_rx)/N;
    

# Replace zero BERs with a small positive number for log plotting
BER_tx[BER_tx == 0] = np.finfo(float).eps;
BER_rx[BER_rx == 0] = np.finfo(float).eps;

# Plot BER comparison
plt.figure();
plt.semilogy(SNR_dB, BER_tx,'-o',linewidth=2);
plt.semilogy(SNR_dB, BER_rx,'-s',linewidth=2);

plt.grid(True);
plt.xlabel('SNR (dB)');
plt.ylabel('Bit Error Rate (BER)');
plt.title('Transmitter vs Receiver Diversity');
plt.legend(['Transmitter Diversity','Receiver Diversity']);
plt.show()
```

**Output:**
<img width="806" height="557" alt="image" src="https://github.com/user-attachments/assets/dd23f8a4-86c2-494c-82b8-54bb3d67d055" />

**Result:**
  Thus, the MATLAB simulation for comparing transmitter diversity and receiver diversity performance was successfully implemented and analyzed.


