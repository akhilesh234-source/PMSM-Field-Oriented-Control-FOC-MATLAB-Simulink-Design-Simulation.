from pathlib import Path
import matplotlib.pyplot as plt
from matplotlib.patches import FancyBboxPatch, FancyArrowPatch
from matplotlib import rcParams

base = Path("/mnt/data/pmsm_foc_project")
img_dir = base / "docs" / "images"
img_dir.mkdir(parents=True, exist_ok=True)

# Create a clean, GitHub-friendly control architecture diagram.
fig, ax = plt.subplots(figsize=(18, 10))
ax.set_xlim(0, 18)
ax.set_ylim(0, 10)
ax.axis("off")

def box(x, y, w, h, title, subtitle=""):
    patch = FancyBboxPatch(
        (x, y), w, h,
        boxstyle="round,pad=0.08,rounding_size=0.12",
        linewidth=1.5, edgecolor="black", facecolor="white"
    )
    ax.add_patch(patch)
    ax.text(x + w/2, y + h*0.64, title, ha="center", va="center",
            fontsize=11, fontweight="bold", wrap=True)
    if subtitle:
        ax.text(x + w/2, y + h*0.28, subtitle, ha="center", va="center",
                fontsize=8.5, wrap=True)

def arrow(x1, y1, x2, y2, label=None, rad=0.0):
    p = FancyArrowPatch((x1, y1), (x2, y2), arrowstyle="-|>",
                        mutation_scale=14, linewidth=1.25,
                        connectionstyle=f"arc3,rad={rad}", color="black")
    ax.add_patch(p)
    if label:
        ax.text((x1+x2)/2, (y1+y2)/2 + 0.16, label,
                ha="center", va="bottom", fontsize=8)

# Main forward control path
box(0.35, 7.4, 1.65, 1.05, "Speed reference", "ω*")
box(2.45, 7.4, 1.8, 1.05, "Speed PI", "ω* − ω")
box(4.75, 7.4, 1.75, 1.05, "q-axis current ref.", "i*q")
box(7.0, 8.15, 1.65, 1.05, "d-axis current PI", "id* − id")
box(7.0, 6.55, 1.65, 1.05, "q-axis current PI", "iq* − iq")
box(9.25, 7.35, 1.8, 1.05, "Inverse Park", "vd, vq, θe")
box(11.55, 7.35, 1.65, 1.05, "SVPWM", "T_a, T_b, T_c")
box(13.7, 7.35, 1.75, 1.05, "3-phase inverter", "6 power switches")
box(15.95, 7.35, 1.7, 1.05, "PMSM", "3-phase motor")

arrow(2.0, 7.93, 2.45, 7.93)
arrow(4.25, 7.93, 4.75, 7.93)
arrow(6.5, 7.93, 7.0, 7.93, "iq*")
arrow(8.65, 8.67, 9.25, 8.05, "vd")
arrow(8.65, 7.07, 9.25, 7.7, "vq")
arrow(11.05, 7.88, 11.55, 7.88)
arrow(13.2, 7.88, 13.7, 7.88)
arrow(15.45, 7.88, 15.95, 7.88)

# Current feedback path
box(13.8, 4.75, 1.65, 1.0, "Current sensing", "ia, ib, ic")
box(11.45, 4.75, 1.65, 1.0, "Clarke transform", "iα, iβ")
box(9.1, 4.75, 1.65, 1.0, "Park transform", "id, iq, θe")
arrow(16.8, 7.35, 14.65, 5.75, "phase currents", rad=0.12)
arrow(13.8, 5.25, 13.1, 5.25)
arrow(11.45, 5.25, 10.75, 5.25)
arrow(9.9, 5.75, 8.0, 6.55, "iq feedback", rad=-0.08)
arrow(9.6, 5.75, 8.0, 8.15, "id feedback", rad=0.16)

# Hall sensor feedback
box(14.0, 2.65, 1.65, 1.0, "Hall sensors", "H_a, H_b, H_c")
box(11.45, 2.65, 1.95, 1.0, "Position / speed", "sector, θe, ω")
arrow(16.8, 7.35, 14.85, 3.65, "Hall signals", rad=0.18)
arrow(14.0, 3.15, 13.4, 3.15)
arrow(12.4, 3.65, 10.1, 4.75, "θe", rad=-0.1)
arrow(12.0, 3.15, 3.35, 7.4, "speed feedback ω", rad=-0.22)
arrow(12.4, 3.65, 10.15, 7.35, "electrical angle θe", rad=0.1)

ax.text(9, 9.65, "PMSM Field-Oriented Control (FOC) — Closed-Loop Architecture",
        ha="center", va="center", fontsize=17, fontweight="bold")
ax.text(9, 1.35,
        "Forward path: speed command → current references → d/q current regulation → inverse Park → SVPWM → inverter → PMSM\n"
        "Feedback: phase-current sensing → Clarke/Park → id, iq; Hall sensors → rotor sector/position and speed",
        ha="center", va="center", fontsize=10)
ax.text(9, 0.55,
        "Conceptual architecture for a Hall-sensor-based PMSM FOC simulation; signal conditioning, limits, protection and sampling logic are implementation-dependent.",
        ha="center", va="center", fontsize=8.5, style="italic")

fig.tight_layout()
diagram_path = img_dir / "pmsm_foc_architecture.png"
fig.savefig(diagram_path, dpi=200, bbox_inches="tight")
plt.close(fig)

readme = r'''# PMSM Field-Oriented Control (FOC) Using MATLAB/Simulink

A modular study and simulation project for a three-phase Permanent Magnet Synchronous Motor (PMSM) drive using **Hall-sensor-based Field-Oriented Control (FOC)**, cascaded PI control, Clarke/Park transformations and Space Vector Pulse Width Modulation (SVPWM).

> **Project status:** This repository provides MATLAB function modules, validation/visualization utilities and detailed instructions for constructing the Simulink model. The referenced project structure identifies `models/PMSM_FOC_Hall_Model.slx` as a model to be created; do not assume a prebuilt `.slx` model is included unless you have added and tested it yourself.

---

## 1. Project Overview

Field-Oriented Control is a vector-control technique that regulates the torque-producing and flux-producing components of stator current independently. Three-phase stator currents are transformed into a rotating reference frame aligned with the rotor magnetic field. In this frame, the current components become approximately DC quantities in steady state and can be regulated using PI controllers.

This project studies a cascaded PMSM drive:

- **Outer speed loop:** compares reference speed with measured speed and generates the q-axis current reference.
- **Inner current loops:** regulate direct-axis current (`id`) and quadrature-axis current (`iq`) independently.
- **Coordinate transformations:** convert measured phase currents from abc to αβ and then to dq coordinates.
- **Modulation and power stage:** inverse Park transformation and SVPWM generate inverter switching commands.
- **Rotor feedback:** three Hall sensors provide six valid electrical sectors per electrical revolution; position and speed are derived from the Hall transitions.

### Project objectives

1. Develop a parameterized electrical and mechanical model of a three-phase PMSM.
2. Implement the FOC signal chain using modular MATLAB functions.
3. Design and tune cascaded PI controllers for speed and current regulation.
4. Generate three-phase inverter duty cycles using SVPWM.
5. Evaluate speed tracking, phase-current quality, torque response and transient behaviour.
6. Validate individual software modules before system-level simulation.

---

## 2. System Architecture

![PMSM FOC closed-loop architecture](docs/images/pmsm_foc_architecture.png)

**Figure 1.** Conceptual closed-loop architecture for Hall-sensor-based PMSM FOC. The exact placement of ADC sampling, filtering, saturation, dead-time compensation and protection depends on the final implementation.

### 2.1 Main signal flow

**Forward control path**

1. The speed reference is compared with measured motor speed.
2. The speed PI controller generates the q-axis current reference (`iq*`).
3. The d-axis and q-axis current controllers compare their references with measured `id` and `iq`.
4. The current PI controllers generate voltage references (`vd*`, `vq*`).
5. Inverse Park transformation converts the dq voltage command into stationary-frame αβ voltage commands.
6. SVPWM calculates the three inverter duty cycles.
7. The three-phase inverter applies the corresponding phase voltages to the PMSM.

**Feedback path**

1. Phase currents (`ia`, `ib`, `ic`) are measured.
2. Clarke transformation converts abc currents into stationary αβ components.
3. Park transformation uses the electrical rotor angle to calculate `id` and `iq`.
4. Hall sensor transitions are decoded to estimate rotor electrical position and speed.
5. Current and speed feedback close the inner and outer control loops.

---

## 3. PMSM Mathematical Model

The PMSM model contains electrical and mechanical subsystems. The following equations use a common rotor-oriented dq convention. Confirm the sign convention and scaling used by your MATLAB functions before combining independently developed modules.

### 3.1 Electrical model in the dq reference frame

For a PMSM:

\[
v_d = R_s i_d + \frac{d\psi_d}{dt} - \omega_e \psi_q
\]

\[
v_q = R_s i_q + \frac{d\psi_q}{dt} + \omega_e \psi_d
\]

where:

- `vd`, `vq` = d-axis and q-axis stator voltages (V)
- `id`, `iq` = d-axis and q-axis stator currents (A)
- `Rs` = stator phase resistance (Ω)
- `ψd`, `ψq` = d-axis and q-axis flux linkages (Wb)
- `ωe` = rotor electrical angular speed (rad/s)

For a PMSM with constant inductances:

\[
\psi_d = L_d i_d + \lambda_m
\]

\[
\psi_q = L_q i_q
\]

where `Ld` and `Lq` are the d-axis and q-axis inductances, and `λm` is the permanent-magnet flux linkage.

Substituting the flux equations:

\[
v_d = R_s i_d + L_d\frac{di_d}{dt} - \omega_e L_q i_q
\]

\[
v_q = R_s i_q + L_q\frac{di_q}{dt}
      + \omega_e(L_d i_d+\lambda_m)
\]

The cross-coupling terms become important at higher electrical speed. A basic PI implementation may initially omit explicit decoupling; a more advanced implementation can compensate these terms.

### 3.2 Electromagnetic torque

For a PMSM:

\[
T_e = \frac{3}{2}p(\psi_d i_q-\psi_q i_d)
\]

or, using the flux-linkage model:

\[
T_e = \frac{3}{2}p[\lambda_m i_q+(L_d-L_q)i_d i_q]
\]

where `p` is the number of pole pairs.

For a surface-mounted PMSM where `Ld ≈ Lq`, the reluctance-torque term is small. With `id* = 0`, torque is primarily controlled by `iq`.

### 3.3 Mechanical model

\[
J\frac{d\omega_m}{dt}=T_e-T_L-B\omega_m
\]

\[
\frac{d\theta_m}{dt}=\omega_m
\]

where:

- `J` = combined rotor/load inertia (kg·m²)
- `B` = viscous friction coefficient (N·m·s/rad)
- `TL` = load torque (N·m)
- `ωm` = mechanical angular speed (rad/s)
- `θm` = mechanical rotor position (rad)

Electrical and mechanical speed are related by:

\[
\omega_e=p\omega_m
\]

Electrical and mechanical rotor angles are related by:

\[
\theta_e=p\theta_m
\]

Use consistent units throughout the model. Convert RPM to rad/s using:

\[
\omega_m=\frac{2\pi n}{60}
\]

---

## 4. Coordinate Transformations

Coordinate transformations make three-phase current control easier by expressing the stator quantities in stationary and rotor-synchronous reference frames.

> **Implementation note:** Clarke/Park matrices are available in different scaling and axis conventions. The functions used in the forward and inverse paths must use mutually consistent conventions.

### 4.1 Clarke transformation (abc → αβ)

For a balanced three-wire system where `ia + ib + ic = 0`, an amplitude-invariant Clarke transformation can be written as:

\[
i_\alpha=i_a
\]

\[
i_\beta=\frac{1}{\sqrt{3}}(i_a+2i_b)
\]

The zero-sequence component is omitted for the balanced three-wire case.

### 4.2 Park transformation (αβ → dq)

Using the convention adopted in this README:

\[
i_d=i_\alpha\cos\theta_e+i_\beta\sin\theta_e
\]

\[
i_q=-i_\alpha\sin\theta_e+i_\beta\cos\theta_e
\]

The electrical rotor angle `θe` is required for this transformation.

### 4.3 Inverse Park transformation (dq → αβ)

\[
v_\alpha=v_d\cos\theta_e-v_q\sin\theta_e
\]

\[
v_\beta=v_d\sin\theta_e+v_q\cos\theta_e
\]

These stationary-frame voltage commands are passed to the SVPWM algorithm.

---

## 5. Hall Sensor-Based Rotor Position Estimation

The project uses three digital Hall sensors to obtain rotor-position information.

### 5.1 Hall sensor operation

- Three Hall sensors are positioned with 120° electrical displacement.
- Their outputs form a three-bit digital state.
- Six valid states occur over one electrical revolution in normal operation.
- Each state corresponds to a 60° electrical sector.
- The Hall transitions provide coarse rotor-position information.

Typical valid states are often represented by a six-state sequence such as `001`, `101`, `100`, `110`, `010`, `011`. The actual order depends on motor wiring, sensor placement, direction of rotation and phase convention. Confirm the sequence experimentally or from the motor documentation.

### 5.2 Position estimation

A basic Hall-based estimator maps each valid Hall state to its corresponding electrical sector. A sector-centre estimate has a quantization uncertainty of approximately ±30 electrical degrees.

For smoother FOC operation, the estimated angle may be interpolated between Hall transitions using the measured transition period and direction. This interpolation is an enhancement and must be implemented and validated; it should not be assumed to exist merely because Hall decoding is present.

### 5.3 Speed estimation

If the elapsed time between equivalent Hall transitions is `Δt`, the electrical angular speed can be estimated from the sector displacement:

\[
\omega_e \approx \frac{\Delta\theta_e}{\Delta t}
\]

For a 60° electrical sector:

\[
\Delta\theta_e=\frac{\pi}{3}\ \text{rad}
\]

Mechanical speed is:

\[
\omega_m=\frac{\omega_e}{p}
\]

The estimator should handle startup, invalid Hall states, direction reversal, timeout at standstill and noisy transitions.

---

## 6. Cascaded PI Controller Design

The control architecture contains one outer speed loop and two inner current loops.

### 6.1 Speed controller

The speed error is:

\[
e_\omega=\omega^*-\omega_m
\]

A PI controller generates the q-axis current reference:

\[
i_q^*=K_{p\omega}e_\omega+
K_{i\omega}\int e_\omega\,dt
\]

The q-axis current reference must be limited to the permitted motor and inverter current range.

### 6.2 d-axis current controller

For a surface PMSM operating below base speed, a common initial reference is:

\[
i_d^*=0
\]

The d-axis current error is:

\[
e_d=i_d^*-i_d
\]

The PI controller generates the d-axis voltage command:

\[
v_d^*=K_{pd}e_d+K_{id}\int e_d\,dt
\]

### 6.3 q-axis current controller

The q-axis current error is:

\[
e_q=i_q^*-i_q
\]

The PI controller generates the q-axis voltage command:

\[
v_q^*=K_{pq}e_q+K_{iq}\int e_q\,dt
\]

### 6.4 Practical tuning sequence

1. Verify the motor parameters, current-sensor scaling, angle convention and inverter polarity.
2. Tune the d-axis and q-axis current loops before tuning the speed loop.
3. Check current-loop stability and transient response over the intended operating range.
4. Tune the speed loop with the current loops closed.
5. Add output saturation and anti-windup to the PI controllers.
6. Verify the response under speed steps and load disturbances.

The speed loop is normally slower than the current loops. Select bandwidths based on the motor, inverter, sampling frequency, delays and stability margins; do not copy PI gains from another motor without recalculation.

---

## 7. Space Vector PWM (SVPWM)

SVPWM converts the requested stationary-frame voltage vector into three-phase inverter duty cycles.

### 7.1 Purpose

- Generates three-phase PWM commands from the αβ voltage reference.
- Uses the available DC-link voltage effectively.
- Produces lower harmonic distortion than basic sinusoidal PWM in many operating conditions.
- Provides a natural interface between the FOC voltage commands and a two-level three-phase inverter.

### 7.2 SVPWM implementation outline

1. Read `vα` and `vβ`.
2. Determine the reference-vector sector.
3. Calculate the active-vector and zero-vector dwell times.
4. Convert dwell times into phase duty cycles.
5. Apply duty-cycle limits and ensure valid switching states.
6. Send the duty cycles to the three-phase inverter PWM generator.

For a two-level inverter, the switching sequence must avoid simultaneous turn-on of the upper and lower switches in the same leg. Practical hardware also requires dead time, gate-driver interlocks and fault handling.

---

## 8. MATLAB Project Structure

The reference project is organized as follows:

```text
pmsm_foc_model/
│
├── README.md
│
├── docs/
│   ├── PMSM_FOC_Theory_and_Implementation.md
│   ├── Simulink_Model_Build_Guide.md
│   └── images/
│       └── pmsm_foc_architecture.png
│
├── scripts/
│   ├── motor_parameters.m
│   ├── hall_sensor_model.m
│   ├── coordinate_transforms.m
│   ├── pi_controller.m
│   └── svpwm.m
│
└── models/
    └── PMSM_FOC_Hall_Model.slx  (to be created and validated)
```

### 8.1 Module responsibilities

| File | Purpose |
|---|---|
| `motor_parameters.m` | Defines motor, controller and simulation parameters |
| `hall_sensor_model.m` | Models Hall sensor states and position/speed estimation |
| `coordinate_transforms.m` | Implements Clarke, Park and inverse transformations |
| `pi_controller.m` | Implements PI control functions |
| `svpwm.m` | Calculates SVPWM duty cycles |
| `PMSM_FOC_Hall_Model.slx` | System-level Simulink model to be constructed |

The repository documentation also describes validation and visualization functions. Confirm that each function exists in your local checkout before running it.

---

## 9. Software Requirements

### Required

- MATLAB R2019b or later (R2021a or later recommended by the reference project).
- Simulink.

### Recommended

- Simscape Electrical for detailed electrical machine and inverter modelling.
- Control System Toolbox for linear analysis and controller design, if used.
- MATLAB scripts for parameter initialization, validation and result plotting.

Some advanced MathWorks motor-control examples require additional products such as Motor Control Blockset, Embedded Coder or hardware-specific blocksets. Those are not automatically required for a basic custom Simulink model.

---

## 10. Getting Started

### 10.1 Clone the repository

```bash
git clone <your-repository-url>
cd pmsm_foc_model
```

### 10.2 Add the scripts folder to the MATLAB path

```matlab
addpath('scripts');
savepath;
```

### 10.3 Load motor and controller parameters

```matlab
motor_parameters;
```

The parameter initialization script should define the motor electrical and mechanical parameters, controller gains and simulation settings.

### 10.4 Validate individual modules

Run the validation functions that are available in your checkout:

```matlab
validate_transformations();
validate_pi_controller();
validate_svpwm();
```

Visualize the Hall sensor sequence and transformation results using the corresponding plotting utilities if they are present.

### 10.5 Construct the Simulink model

Follow:

```text
docs/Simulink_Model_Build_Guide.md
```

Build and connect the following subsystems:

1. PMSM electrical and mechanical plant.
2. Three-phase inverter.
3. Phase-current measurement.
4. Hall sensor and rotor position estimator.
5. Clarke and Park transformations.
6. Speed and current PI controllers.
7. Inverse Park transformation.
8. SVPWM and inverter gate-signal generation.
9. Feedback, limits and protection logic.

Save the completed model as:

```text
models/PMSM_FOC_Hall_Model.slx
```

### 10.6 Run simulation

After the model has been built and all parameters are initialized:

```matlab
open_system('models/PMSM_FOC_Hall_Model.slx');
sim('PMSM_FOC_Hall_Model');
```

Inspect the speed, current, torque, Hall-state and PWM signals using Scopes or logged simulation data.

---

## 11. Motor Parameter Configuration

Use the actual motor datasheet or parameter-identification results. The reference project gives the following illustrative parameter values:

| Parameter | Example value | Description |
|---|---:|---|
| `Rs` | 0.285 Ω | Stator resistance |
| `Ls` | 0.00085 H | Stator inductance in the simplified model |
| `lambda_m` | 0.0118 Wb | PM flux linkage |
| `P` | 4 | Pole pairs |
| `J` | 0.00001 kg·m² | Moment of inertia |
| `B` | 0.00001 N·m·s | Viscous friction coefficient |

These values are examples from the reference project, not universal PMSM parameters. For an IPMSM, use separate `Ld` and `Lq` values. Confirm whether the code expects phase, line-to-line, peak, RMS, mechanical or electrical quantities.

---

## 12. Simulation and Validation Test Plan

The following tests should be run after the complete model is assembled.

### Test 1 — No-load startup

**Objective:** Verify startup and speed-reference tracking.

- Start the motor from zero speed.
- Apply a constant speed reference.
- Observe acceleration, overshoot and settling.
- Verify that the current remains within the configured limit.

### Test 2 — Speed reference step

**Objective:** Evaluate dynamic speed tracking.

- Apply a step change in speed reference.
- Observe speed error and q-axis current demand.
- Verify stable settling without sustained oscillations.

### Test 3 — Load torque step

**Objective:** Evaluate disturbance rejection.

- Start at a steady operating speed.
- Apply a step increase in load torque.
- Observe speed dip, q-axis current response and recovery.
- Verify that the inverter and motor current limits are respected.

### Test 4 — Current regulation

**Objective:** Verify decoupled d-axis and q-axis control.

- Observe `id` and `iq`.
- Confirm that `id` tracks its reference.
- Confirm that `iq` follows the torque-producing current command.
- Check current-loop stability and saturation behaviour.

### Test 5 — Hall sensor sequence

**Objective:** Validate rotor-position feedback.

- Observe the three Hall signals.
- Verify six valid states per electrical revolution.
- Check transition order and estimated direction.
- Confirm that invalid states and standstill timeouts are handled.

### Test 6 — SVPWM output

**Objective:** Validate modulation and inverter commands.

- Observe the three duty-cycle commands.
- Verify that duty cycles remain within allowed bounds.
- Check the phase relationship and switching sequence.
- Verify that no inverter leg receives overlapping upper/lower gate commands.

---

## 13. Performance Metrics

Measure performance from the actual simulation results. The reference project lists the following as achievable targets with suitable motor parameters and controller tuning; they are not guaranteed results.

| Metric | Reference target |
|---|---:|
| Speed tracking error | < 1% |
| Rise time | 100–200 ms |
| Settling time | 200–300 ms |
| Overshoot | < 5% |
| Current-loop bandwidth | 1–2 kHz |
| Speed-loop bandwidth | 50–200 Hz |

Report the actual values from your simulation, including the reference command, load condition, solver settings and controller gains.

Recommended plots:

1. Reference speed and measured speed.
2. d-axis and q-axis current references and measured currents.
3. Three-phase stator currents (`ia`, `ib`, `ic`).
4. Electromagnetic torque and load torque.
5. Hall sensor digital outputs and estimated rotor angle.
6. SVPWM duty cycles and inverter phase voltages.

---

## 14. Controller Tuning and Troubleshooting

| Observation | Possible cause | Suggested check |
|---|---|---|
| Motor does not start | Incorrect Hall sequence or phase mapping | Verify phase wiring and Hall-state order |
| Speed does not track | Incorrect speed-loop gains or feedback scaling | Check units, polarity and PI gains |
| Current oscillates | Current-loop gains too high or noisy feedback | Check gains, sampling and current filtering |
| Non-zero steady-state error | Insufficient integral action or saturation | Check integral gain and anti-windup |
| Torque ripple | Position quantization or current measurement error | Check Hall interpolation and current sensing |
| Duty cycle saturates | Requested voltage exceeds available DC-link voltage | Check DC bus, speed and voltage limiting |
| NaN/Inf in simulation | Invalid initial condition or numerical instability | Check parameter values and solver settings |
| Algebraic loop | Direct feedthrough in feedback path | Review subsystem delays and discrete implementation |

Tune the current loops before the speed loop. Apply integrator limits and anti-windup so that the controller recovers correctly after voltage or current saturation.

---

## 15. Possible Extensions

The following are potential improvements and should be treated as future work until implemented and tested.

- **Sensorless FOC:** Replace Hall-based rotor feedback with a back-EMF observer, sliding-mode observer or other validated estimator.
- **Field weakening:** Introduce negative d-axis current above base speed, subject to voltage and current constraints.
- **Maximum Torque Per Ampere (MTPA):** Optimize current references for an interior PMSM.
- **Dead-time compensation:** Reduce inverter-induced voltage error and current distortion.
- **ADC and PWM synchronization:** Sample phase currents at suitable points in the PWM cycle and coordinate ADC triggering with the control interrupt.
- **Embedded deployment:** Generate and validate code for a target MCU/DSP, including ADC, PWM, timer, fault and protection peripherals.

---

## 16. Relevance to Embedded Motor Control and Silicon Validation

This project provides a software-level understanding of the control functions commonly used in embedded motor drives:

- ADC-based phase-current and DC-link voltage acquisition.
- PWM timer configuration and duty-cycle updates.
- Hall sensor digital-input capture and rotor-position estimation.
- Real-time execution of Clarke/Park transforms and PI controllers.
- SVPWM generation and inverter control.
- Current limiting, fault detection and protection.

A simulation does not establish silicon-level ADC accuracy, peripheral timing, interrupt latency or hardware protection performance. Those require target-hardware testing and measurement.

---

## 17. References and Further Reading

1. **NXP Semiconductors — AN12235: 3-Phase Sensorless PMSM Motor Control Kit with S32K144**  
   https://www.nxp.com/docs/en/application-note/AN12235.pdf

2. **MathWorks — Field-Oriented Control of PMSM Using Hall Sensor**  
   https://www.mathworks.com/help/mcb/gs/foc-pmsm-using-hall-sensor-example.html

3. **Microchip — Sensorless Field-Oriented Control of a PMSM (AN1078)**  
   https://ww1.microchip.com/downloads/en/AppNotes/01078B.pdf

4. **Infineon — FM3 Family 3-Phase PMSM FOC Control**  
   https://www.infineon.com/assets/row/public/documents/30/42/infineon-an204469-fm3-family-3-phase-pmsm-foc-control-applicationnotes-en.pdf

5. R. Krishnan, *Permanent Magnet Synchronous and Brushless DC Motor Drives*.

6. D. W. Novotny and T. A. Lipo, *Vector Control and Dynamics of AC Drives*.

---

## 18. Project Disclaimer

This is an educational motor-control design and simulation project. The controller, motor parameters, inverter model, Hall sequence and protection logic must be validated for the selected motor and hardware before any physical implementation. Do not connect an unvalidated control model to a power inverter or motor.

## 19. License

The reference project describes its contents as educational material. Retain the original repository's license and attribution terms when reusing its code. Add a license file to your own repository only after deciding which license applies to your original contributions and checking the upstream code's terms.
'''

readme_path = base / "README.md"
readme_path.write_text(readme, encoding="utf-8")

print(f"Created: {readme_path}")
print(f"Created: {diagram_path}")
print(f"README size: {readme_path.stat().st_size:,} bytes")
print(f"Diagram size: {diagram_path.stat().st_size:,} bytes")
