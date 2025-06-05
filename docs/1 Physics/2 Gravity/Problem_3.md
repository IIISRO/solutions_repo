# Trajectories of a Freely Released Payload Near Earth

## Motivation
When an object is released from a moving rocket near Earth, its trajectory depends on initial conditions and gravitational forces. This scenario presents a rich problem, blending principles of orbital mechanics and numerical methods. Understanding the potential trajectories is vital for space missions, such as deploying payloads or returning objects to Earth.

## Task

1. **Analyze the possible trajectories** (e.g., parabolic, hyperbolic, elliptical) of a payload released near Earth.
2. **Perform a numerical analysis** to compute the path of the payload based on given initial conditions (position, velocity, and altitude).
3. **Discuss** how these trajectories relate to orbital insertion, reentry, or escape scenarios.
4. **Develop a computational tool** to simulate and visualize the motion of the payload under Earth's gravity, accounting for initial velocities and directions.

## 1. Theory of Trajectories

The trajectory of a payload released near Earth can take one of several forms based on its initial velocity:

- **Parabolic Trajectory**: If the payload is launched at an angle with a velocity smaller than escape velocity, it will trace a parabolic path and eventually return to Earth.
  
- **Elliptical Trajectory**: If the velocity is such that the payload follows an elliptical orbit around Earth, it will return to Earth after one full orbit.

- **Hyperbolic Trajectory**: If the velocity exceeds the escape velocity, the payload will escape Earth's gravitational influence and follow a hyperbolic trajectory.

### Gravitational Force

The gravitational force acting on the payload is governed by Newton's law of gravitation:

$$
F = \frac{GMm}{r^2}
$$

Where:
- $G$ is the gravitational constant ($6.67430 \times 10^{-11} \, \text{m}^3 \text{kg}^{-1} \text{s}^{-2}$)
- $M$ is Earth's mass ($5.972 \times 10^{24} \, \text{kg}$)
- $m$ is the payload's mass (which we will assume to be constant)
- $r$ is the distance between the center of the Earth and the payload

## 2. Numerical Simulation

We'll perform a numerical simulation to model the payload's motion using **Newton's Law of Gravitation**. We will use **Euler's method** or **Runge-Kutta method** for time integration to compute the payload's trajectory.

### Initial Conditions

For the simulation, we assume the following initial conditions:
- Initial position: $r_0 = 10,000 \, \text{km}$ (10,000 kilometers from Earth's surface)
- Initial velocity: This will be varied to study different trajectory types (parabolic, elliptical, hyperbolic)

### Numerical Solution

We will use the following equations of motion to update the payload's position and velocity over time:

$$
\vec{F} = -\frac{GMm}{r^2} \hat{r}
$$

Where:
- $\vec{F}$ is the gravitational force vector
- $\hat{r}$ is the unit vector pointing radially outward from Earth

Using **Euler’s method**, the position and velocity are updated as:

$$
\vec{r}(t + \Delta t) = \vec{r}(t) + \vec{v}(t) \Delta t
$$

$$
\vec{v}(t + \Delta t) = \vec{v}(t) + \vec{a}(t) \Delta t
$$

Where:
- $\vec{r}(t)$ is the position vector at time $t$
- $\vec{v}(t)$ is the velocity vector at time $t$
- $\vec{a}(t)$ is the acceleration due to gravity at time $t$

## 3. Numerical Code (Python)

```python
# Install imageio if needed
!pip install imageio

import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
import imageio
from IPython.display import Image

# Constants
G = 6.67430e-11  # m^3 kg^-1 s^-2
M_earth = 5.972e24  # kg
R_earth = 6371e3  # meters

# Initial settings
altitude = 800e3  # meters
initial_position = np.array([R_earth + altitude, 0])  # Right side
velocities = np.arange(5e3, 13.5e3, 0.5e3)  # from 5 km/s to 13 km/s

dt = 5  # seconds
steps = 3000  # number of steps

def gravity_accel(r):
    r_mag = np.linalg.norm(r)
    return -G * M_earth * r / r_mag**3

# Simulate trajectories
trajectories = []

for v_mag in velocities:
    r = initial_position.copy()
    v = np.array([0, v_mag])  # Shoot vertically upward
    path = [r.copy()]
    for _ in range(steps):
        a = gravity_accel(r)
        v += a * dt
        r += v * dt
        if np.linalg.norm(r) < R_earth:
            break  # impact
        path.append(r.copy())
    trajectories.append(np.array(path))

# Setup figure for animation
fig, ax = plt.subplots(figsize=(8, 8))
ax.set_facecolor("black")
limit = R_earth + 12000e3
ax.set_xlim(-limit, limit)
ax.set_ylim(-limit, limit)
ax.set_aspect('equal')
ax.axis('off')

# Draw Earth
earth = plt.Circle((0, 0), R_earth, color='blue', alpha=0.9)
ax.add_artist(earth)

# Plot elements to update
lines = [ax.plot([], [], lw=1.5)[0] for _ in velocities]
colors = plt.cm.plasma(np.linspace(0, 1, len(velocities)))

for line, color in zip(lines, colors):
    line.set_color(color)

def init():
    for line in lines:
        line.set_data([], [])
    return lines

def update(frame):
    for i, traj in enumerate(trajectories):
        if frame < len(traj):
            lines[i].set_data(traj[:frame, 0], traj[:frame, 1])
    return lines

ani = FuncAnimation(fig, update, frames=1000, init_func=init, blit=True)

# Save as gif
gif_path = "payload_trajectories.gif"
ani.save(gif_path, writer='pillow', fps=30)

# Display gif
Image(filename=gif_path)
```
![alt text](https://github.com/IIISRO/solutions_repo/raw/refs/heads/main/docs/media/payload_trajectories.gif)
![alt text](https://github.com/IIISRO/solutions_repo/raw/refs/heads/main/docs/media/one_by_one_trajectories.gif)

[colab](https://colab.research.google.com/drive/194VxVh5Z1fKNtWXRRy1Vv7nMQ2MVjSfA?usp=sharing)

###  Code Explanation

The code provided simulates the trajectory of a payload released near Earth using Newton's Law of Gravitation. Here's a breakdown of each section:

### Constants:
- **G**: Gravitational constant $6.67430 \times 10^{-11} \, \text{m}^3 \, \text{kg}^{-1} \, \text{s}^{-2}$
- **M**: Mass of Earth $5.972 \times 10^{24} \, \text{kg}$
- **R_earth**: Radius of Earth $6.371 \times 10^6 \, \text{m}$

### Initial Conditions:
- **r0**: Initial position, which is set to 10,000 km above Earth's surface. This value is converted into meters.
- **v0**: Initial velocity of the payload, which is assumed to be 1000 m/s for demonstration purposes.
- **theta**: Launch angle, set to 45 degrees (π/4 radians), which can be varied to simulate different trajectories.

### Time Step and Simulation:
- **dt**: Time step for each iteration (10 seconds).
- **T_total**: Total time for the simulation, set to 10,000 seconds for an initial test run.
- **time_steps**: Total number of time steps, calculated by dividing the total time by the time step.

### Numerical Integration:
- **Euler's method** is used to update the position and velocity of the payload over time:
  - The acceleration due to gravity is calculated as:
    $$
    \vec{a} = - \frac{GM}{r^2} \hat{r}
    $$
    where $r$ is the distance between the Earth and the payload.
  - The velocity and position of the payload are updated iteratively using the equations:
    $$
    \vec{r}(t + \Delta t) = \vec{r}(t) + \vec{v}(t) \Delta t
    $$
    $$
    \vec{v}(t + \Delta t) = \vec{v}(t) + \vec{a}(t) \Delta t
    $$

### Plotting:
- The trajectory of the payload is plotted using **matplotlib**, showing the path taken relative to Earth's center (represented by a yellow circle at the origin).
- The x and y axes represent the position of the payload in meters, and the trajectory is plotted on a 2D plane with Earth at the center.
  
---

## 4. Real-World Applications

Understanding the trajectory of a freely released payload near Earth is essential for numerous space exploration applications:

- **Orbital Insertion**: The ability to calculate the trajectory is crucial when placing satellites into orbit. By adjusting the payload's velocity and trajectory, we can ensure that satellites remain in stable orbits around Earth.
  
- **Reentry**: Understanding the trajectory is vital for planning the reentry of objects back into Earth's atmosphere. A payload must be correctly placed in an orbit to return to Earth at a controlled reentry speed, minimizing damage upon landing.
  
- **Escape Scenarios**: When sending objects beyond Earth's gravitational influence, such as interplanetary probes or missions to other celestial bodies, it is crucial to calculate the escape velocity and the corresponding trajectory to ensure that the object doesn't fall back to Earth. 

- **Space Missions and Planetary Exploration**: Accurate trajectory analysis is essential for planetary missions, such as sending payloads to other planets or moons. By understanding how the initial velocity impacts the trajectory, space agencies can better design missions to explore the solar system and beyond.

---

## Conclusion

In this task, we analyzed the motion of a freely released payload near Earth using Newton's Law of Gravitation. By applying numerical integration methods, specifically **Euler's method**, we simulated and visualized the trajectory of the payload under Earth's gravitational influence.

We discussed three possible types of trajectories:
- **Parabolic**: Payload returns to Earth after a single trajectory.
- **Elliptical**: Payload follows an elliptical orbit around Earth.
- **Hyperbolic**: Payload escapes Earth's gravitational pull and follows a hyperbolic trajectory.

The **real-world applications** of this analysis are crucial for space exploration, particularly in areas like satellite deployment, orbital insertion, reentry planning, and interplanetary missions.

Through the simulation and visualizations, we gained insights into the dynamics of payload trajectories and the effects of initial velocity and launch angle on the motion of objects near Earth. This understanding is fundamental for mission planning, ensuring safe and successful space exploration operations.
