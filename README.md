# Guard_key_kalman-
Kalman Filter + Guard Key simulation for sensor data smoothing and safe state estimation. Includes plots and final estimate output for client-ready demonstration.
# kalman_guard_key.py
"""
Client: Lil Mouse
Project: Kalman Filter + Guard Key Simulation
Purpose: Demonstrate sensor data filtering and state estimation
Run:
    python kalman_guard_key.py
"""

import numpy as np
import matplotlib.pyplot as plt

# ----------------------------
# 1. Simulated Sensor Data
# ----------------------------
np.random.seed(42)
true_position = 1.5      # meters
num_measurements = 50
measurement_noise = 0.1
measurements = true_position + measurement_noise * np.random.randn(num_measurements)

# ----------------------------
# 2. Kalman Filter Setup
# ----------------------------
x = 0.0          # initial estimate
P = 1.0          # initial uncertainty
Q = 0.01         # process noise
R = measurement_noise**2  # measurement noise

x_estimates = []

for z in measurements:
    # Predict (KF Step)
    x_pred = x
    P_pred = P + Q

    # Update (KF Step)
    K = P_pred / (P_pred + R)
    x = x_pred + K * (z - x_pred)
    P = (1 - K) * P_pred

    # Guard Key: ensure estimate is within realistic bounds
    if x < 0:
        x = 0
    elif x > 5:
        x = 5

    x_estimates.append(x)

# ----------------------------
# 3. Plot Results
# ----------------------------
plt.figure(figsize=(8,5))
plt.plot(range(num_measurements), measurements, 'r.', label='Measurements')
plt.plot(range(num_measurements), x_estimates, 'b-', label='KF + Guard Key')
plt.axhline(true_position, color='g', linestyle='--', label='True Position')
plt.title('Kalman Filter + Guard Key Simulation')
plt.xlabel('Measurement Index')
plt.ylabel('Position (m)')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

# ----------------------------
# 4. Print Last Estimate
# ----------------------------
print(f"Final Kalman Filter + Guard Key estimate: {x_estimates[-1]:.3f} m")


<img width="697" height="393" alt="Screenshot 1447-03-09 at 08 45 40" src="https://github.com/user-attachments/assets/3dda9435-0813-4129-839b-8a227d283190" />
Final Kalman Filter + Guard Key estimate: 1.405 m 
