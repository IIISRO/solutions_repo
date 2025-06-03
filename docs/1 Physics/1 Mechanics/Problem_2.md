# Investigating the Dynamics of a Forced Damped Pendulum

## Motivation
The forced damped pendulum is a fascinating example of nonlinear dynamics, exhibiting behavior that ranges from simple periodic motion to chaotic oscillations. This system is governed by the interplay of damping, restoring forces, and external periodic driving, making it a crucial model for understanding diverse physical and engineering systems. Applications include energy harvesting, mechanical resonance, and vibration control.

## Theoretical Foundation
The equation governing a forced damped pendulum is given by:

$$
\frac{d^2\theta}{dt^2} + \beta \frac{d\theta}{dt} + \omega_0^2 \sin(\theta) = A \cos(\Omega t)
$$

where:
- $\theta$ is the angular displacement,
- $\beta$ is the damping coefficient,
- $\omega_0^2 = g/L$ is the natural frequency of the pendulum,
- $A$ is the amplitude of the external driving force,
- $\Omega$ is the driving frequency.

For small angles ($\theta \approx \sin(\theta)$), the equation simplifies to:

$$
\frac{d^2\theta}{dt^2} + \beta \frac{d\theta}{dt} + \omega_0^2 \theta = A \cos(\Omega t)
$$

This approximation allows us to analyze resonance and stability conditions.

## Analysis of Dynamics
To explore the system's dynamics, we numerically solve the equation for various values of $\beta$, $A$, and $\Omega$.

### Key Features:
- **Resonance**: When $\Omega \approx \omega_0$, the pendulum experiences maximum amplitude oscillations.
- **Damping Effects**: Higher $\beta$ leads to reduced oscillations and eventual stabilization.
- **Chaotic Behavior**: For large $A$ and specific $\Omega$, the pendulum exhibits sensitive dependence on initial conditions.

## Implementation
A Python script is developed using the Runge-Kutta method to solve the differential equation numerically. The results are visualized using phase diagrams, Poincaré sections, and bifurcation plots.

### Advanced Python Script with Animation
```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from scipy.integrate import solve_ivp
from IPython.display import HTML

# Define the equation of motion
def forced_damped_pendulum(t, y, beta, omega0, A, Omega):
    theta, omega = y
    dtheta_dt = omega
    domega_dt = -beta * omega - omega0**2 * np.sin(theta) + A * np.cos(Omega * t)
    return [dtheta_dt, domega_dt]

# Parameters
beta = 0.5   # Damping coefficient
omega0 = 1.0  # Natural frequency
A = 1.2      # Driving amplitude
Omega = 0.8  # Driving frequency

t_span = (0, 50)
t_eval = np.linspace(0, 50, 1000)

# Initial conditions
theta0 = 0.2
omega0_init = 0.0

sol = solve_ivp(forced_damped_pendulum, t_span, [theta0, omega0_init], t_eval=t_eval, args=(beta, omega0, A, Omega))

theta_vals = sol.y[0]

length = 1.0  # Length of pendulum
x_vals = length * np.sin(theta_vals)
y_vals = -length * np.cos(theta_vals)

fig, ax = plt.subplots(figsize=(5, 5))
ax.set_xlim(-1.2, 1.2)
ax.set_ylim(-1.2, 1.2)
ax.set_aspect('equal')
ax.set_xticks([])
ax.set_yticks([])
ax.set_title('Forced Damped Pendulum Animation')

line, = ax.plot([], [], 'o-', lw=2, markersize=10, markerfacecolor='red')

def init():
    line.set_data([], [])
    return line,

def update(frame):
    line.set_data([0, x_vals[frame]], [0, y_vals[frame]])
    return line,

ani = animation.FuncAnimation(fig, update, frames=len(x_vals), init_func=init, interval=30, blit=True)

HTML(ani.to_jshtml())
```
<video width="500" controls>
    <source src="https://github.com/IIISRO/solutions_repo/raw/refs/heads/main/docs/media/forceddamped.mp4" type="video/mp4">
</video>

[colab](https://colab.research.google.com/drive/14JCdB3p2wlOL98uMvpiJflb4J5cvPyf0?authuser=0)

## Advanced Visualizations
- **Phase Portraits**: Plot $\omega$ vs. $\theta$ to observe stability and attractors.
- **Poincaré Sections**: Identify chaotic behavior by sampling at integer multiples of the driving period.
- **Bifurcation Diagrams**: Examine transitions from periodic to chaotic motion by varying parameters.

## Practical Applications
- **Energy Harvesting**: Understanding resonance conditions for maximizing energy extraction.
- **Vibration Control**: Engineering applications in bridges and buildings to mitigate oscillatory forces.
- **Nonlinear Circuit Analysis**: Electrical analogs using RLC circuits with periodic driving.

## Conclusion
The forced damped pendulum is an excellent system for studying nonlinear dynamics, resonance, and chaos. By using numerical methods, we gain deeper insights into the effects of forcing and damping, bridging theoretical physics with real-world applications.

## Future Work
- Incorporating nonlinear damping effects.
- Studying the impact of non-periodic or stochastic driving forces.
- Extending analysis to coupled pendulum systems.
```