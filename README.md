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

****4. Develop a MATLAB simulation for user allocation in OFDMA communication.**

**Aim:**
   To develop a MATLAB simulation for user allocation in an OFDMA communication system.

**Objective:**
  To allocate OFDMA subcarriers to users based on channel gain and analyze the resource allocation process.

**Theory:**
   Orthogonal Frequency Division Multiple Access (OFDMA) is a multiple access technique used in modern wireless communication systems such as 4G and 5G. In OFDMA, the available bandwidth is divided into several orthogonal subcarriers, and different subcarriers are assigned to different users.

**Efficient user allocation improves:**

System capacity
Spectral efficiency
Data transmission performance

**In this simulation:****
   Random channel gains are generated for users.
Each subcarrier is allocated to the user with the maximum channel gain.
The allocation matrix shows the assigned subcarriers.

**Code:**
```
import numpy as np
import matplotlib.pyplot as plt

# Parameters
num_users = 4          # Number of users
num_subcarriers = 8    # Number of OFDMA subcarriers

# Generate random channel gains
channel_gain = np.random.rand(num_users, num_subcarriers)

# Display channel gains
print('Channel Gain Matrix:')
print(channel_gain)

# Initialize allocation matrix
allocation = np.zeros((num_users, num_subcarriers))

# Allocate each subcarrier to the user
# having maximum channel gain
for sc_idx in range(num_subcarriers):
    
    # Find best user for current subcarrier (0-indexed)
    user_idx = np.argmax(channel_gain[:, sc_idx])
    
    # Allocate subcarrier
    allocation[user_idx, sc_idx] = 1

# Display allocation result
print('\nSubcarrier Allocation Matrix:')
print(allocation)

# Display user allocation clearly
for sc_idx in range(num_subcarriers):
    
    # np.where returns a tuple of arrays for row and column indices. For a 1D condition,
    # it returns a tuple containing a single array. We need to access that array.
    row_indices = np.where(allocation[:, sc_idx] == 1)[0]
    
    # Since only one user is allocated per subcarrier, take the first (and only) element
    allocated_user = row_indices[0] # This is 0-indexed
    
    # Displaying 1-indexed subcarrier and user for clarity
    print(f'Subcarrier {sc_idx + 1} allocated to User {allocated_user + 1}')

# Plot allocation matrix
plt.figure()
plt.imshow(allocation, cmap='viridis', origin='lower') # origin='lower' makes y-axis start from bottom

plt.xlabel('Subcarriers')
plt.ylabel('Users')
plt.title('OFDMA User Allocation')

plt.colorbar(label='Allocation (1=allocated, 0=not allocated)')
plt.xticks(np.arange(num_subcarriers), np.arange(1, num_subcarriers + 1))
plt.yticks(np.arange(num_users), np.arange(1, num_users + 1))
plt.grid(True, which='both', color='white', linestyle='-', linewidth=0.5)
plt.show()
```

**Output:**

Channel Gain Matrix:
[[0.80633494 0.46677747 0.51535563 0.3916067  0.80081197 0.85998293
  0.87273202 0.428658  ]
 [0.40506825 0.23261563 0.93216923 0.90900839 0.61323237 0.07889641
  0.99745908 0.40078192]
 [0.67211178 0.14249259 0.05648878 0.31989503 0.41784783 0.49047154
  0.33313975 0.64997813]
 [0.8185262  0.59072503 0.79090533 0.31148777 0.48266027 0.69600667
  0.03195787 0.32399206]]

Subcarrier Allocation Matrix:
[[0. 0. 0. 0. 1. 1. 0. 0.]
 [0. 0. 1. 1. 0. 0. 1. 0.]
 [0. 0. 0. 0. 0. 0. 0. 1.]
 [1. 1. 0. 0. 0. 0. 0. 0.]]
Subcarrier 1 allocated to User 4
Subcarrier 2 allocated to User 4
Subcarrier 3 allocated to User 2
Subcarrier 4 allocated to User 2
Subcarrier 5 allocated to User 1
Subcarrier 6 allocated to User 1
Subcarrier 7 allocated to User 2
Subcarrier 8 allocated to User 3
 <img width="792" height="485" alt="image" src="https://github.com/user-attachments/assets/21de0bf2-bb4b-4f15-a6ca-75cf19b71369" />

**Result:**
  Thus, the MATLAB simulation for user allocation in OFDMA communication was successfully implemented and analyzed.

**5. Develop a MATLAB simulation for smart antenna systems in 5G communication**

**Aim:**
  To develop a MATLAB simulation for smart antenna systems used in 5G communication.

**Objective:**
  To simulate beamforming using a smart antenna array and analyze the radiation pattern in a 5G communication system.

**Theory:**
   Smart antenna systems use multiple antenna elements to improve wireless communication performance. In 5G communication, smart antennas help in:
Beamforming
Interference reduction
Improved signal quality
Increased network capacity
Better coverage

Beamforming focuses the transmitted signal toward a desired user direction instead of transmitting equally in all directions.

The antenna array forms a directional radiation pattern using steering vectors.  

**Code:**
```
import numpy as np
import matplotlib.pyplot as plt

# Parameters
N = 8                     # Number of antenna elements
d = 0.5                   # Distance between elements (lambda/2, if d = 0.5)
theta = np.arange(-90, 91, 1) # Angle range in degrees
signal_angle = 20         # Desired signal direction in degrees

# Convert angle to radians
theta_rad = np.deg2rad(theta)
signal_rad = np.deg2rad(signal_angle)

# Steering vector for desired signal
w = np.zeros(N, dtype=complex)
for n in range(N):
    # Python uses 0-based indexing, so (n) in MATLAB is (n-1) in Python
    w[n] = np.exp(-1j * 2 * np.pi * d * n * np.sin(signal_rad))

# Calculate array factor
AF = np.zeros_like(theta_rad, dtype=complex)

for k in range(len(theta_rad)):
    sum_AF = 0 + 0j # Initialize as complex number
    for n in range(N):
        sum_AF = sum_AF + w[n] * np.exp(1j * 2 * np.pi * d * n * np.sin(theta_rad[k]))
    AF[k] = sum_AF

# Take the absolute value and normalize array factor
AF_normalized = np.abs(AF) / np.max(np.abs(AF))

# Plot radiation pattern
plt.figure(figsize=(10, 6))
plt.plot(theta, AF_normalized, linewidth=2)
plt.grid(True)
plt.xlabel('Angle (Degrees)')
plt.ylabel('Normalized Gain')
plt.title('Smart Antenna Beamforming for 5G (Cartesian Plot)')
plt.show()

# Polar plot
plt.figure(figsize=(8, 8))
plt.polar(theta_rad, AF_normalized, linewidth=2)
plt.title('Polar Radiation Pattern')
plt.show()
```

**Output:**

<img width="1003" height="633" alt="image" src="https://github.com/user-attachments/assets/9963c63e-7a9c-4330-aab5-1deffe2c1b5c" />
<img width="896" height="808" alt="image" src="https://github.com/user-attachments/assets/c6acda52-2270-4397-8046-b942cb055433" />

**Result:**
  Thus, the MATLAB simulation for smart antenna systems in 5G communication was successfully implemented and analyzed.
