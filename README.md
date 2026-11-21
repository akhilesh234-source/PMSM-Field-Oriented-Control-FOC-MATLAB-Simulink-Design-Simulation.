
# **PMSM Field-Oriented Control (FOC) Using MATLAB/Simulink**

Motor Drives | Power Electronics | MATLAB | Simulink | Control Systems

## **1. Project Overview**

This project focuses on the modelling, design and simulation of a **Permanent Magnet Synchronous Motor (PMSM) drive using Field-Oriented Control (FOC)** in MATLAB/Simulink.

Field-Oriented Control is an advanced vector-control technique that independently regulates the torque-producing and flux-producing components of stator current. By transforming three-phase stator currents into a rotating d-q reference frame, the PMSM can be controlled similarly to a separately excited DC motor.

The proposed system implements a cascaded control architecture consisting of an outer speed-control loop and inner d-q current-control loops. Hall-effect sensors are used for rotor-position feedback, while Space Vector Pulse Width Modulation (SVPWM) generates switching commands for the three-phase voltage-source inverter.

### **Key features**

- PMSM electrical and mechanical modelling.
- Field-Oriented Control using Clarke and Park transformations.
- Cascaded PI controllers for speed and current regulation.
- Hall-effect sensor-based rotor-position estimation.
- SVPWM-based three-phase inverter control.
- MATLAB-based modular control functions.
- Simulation and validation of speed, current and torque responses.

## **2. System Architecture**

The complete control architecture consists of the following subsystems:

\#chatgpt-mermaid-\_r_18p\_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(237, 237, 237);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-\_r_18p\_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_18p\_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_18p\_ .error-icon{fill:rgb(48, 48, 48);}#chatgpt-mermaid-\_r_18p\_ .error-text{fill:rgb(237, 237, 237);stroke:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_18p\_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-\_r_18p\_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-\_r_18p\_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-\_r_18p\_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-\_r_18p\_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-\_r_18p\_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-\_r_18p\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_18p\_ .marker.cross{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_18p\_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-\_r_18p\_ p{margin:0;}#chatgpt-mermaid-\_r_18p\_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_18p\_ .cluster-label text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_18p\_ .cluster-label span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_18p\_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-\_r_18p\_ .label text,#chatgpt-mermaid-\_r_18p\_ span{fill:rgb(237, 237, 237);color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_18p\_ .node rect,#chatgpt-mermaid-\_r_18p\_ .node circle,#chatgpt-mermaid-\_r_18p\_ .node ellipse,#chatgpt-mermaid-\_r_18p\_ .node polygon,#chatgpt-mermaid-\_r_18p\_ .node path{fill:rgb(9, 23, 44);stroke:rgb(31, 78, 148);stroke-width:1px;}#chatgpt-mermaid-\_r_18p\_ .rough-node .label text,#chatgpt-mermaid-\_r_18p\_ .node .label text,#chatgpt-mermaid-\_r_18p\_ .image-shape .label,#chatgpt-mermaid-\_r_18p\_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-\_r_18p\_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-\_r_18p\_ .rough-node .label,#chatgpt-mermaid-\_r_18p\_ .node .label,#chatgpt-mermaid-\_r_18p\_ .image-shape .label,#chatgpt-mermaid-\_r_18p\_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-\_r_18p\_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-\_r_18p\_ .root .anchor path{fill:rgb(175, 175, 175)!important;stroke-width:0;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_18p\_ .arrowheadPath{fill:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_18p\_ .edgePath .path{stroke:rgb(175, 175, 175);stroke-width:1px;}#chatgpt-mermaid-\_r_18p\_ .flowchart-link{stroke:rgb(175, 175, 175);fill:none;}#chatgpt-mermaid-\_r_18p\_ .edgeLabel{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-\_r_18p\_ .edgeLabel p{background-color:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_18p\_ .edgeLabel rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_18p\_ .labelBkg{background-color:rgba(0, 0, 0, 0.5);}#chatgpt-mermaid-\_r_18p\_ .cluster rect{fill:rgb(48, 48, 48);stroke:rgba(255, 255, 255, 0.15);stroke-width:1px;}#chatgpt-mermaid-\_r_18p\_ .cluster text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_18p\_ .cluster span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_18p\_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(48, 48, 48);border:1px solid rgba(255, 255, 255, 0.15);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-\_r_18p\_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_18p\_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-\_r_18p\_ .icon-shape,#chatgpt-mermaid-\_r_18p\_ .image-shape{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-\_r_18p\_ .icon-shape p,#chatgpt-mermaid-\_r_18p\_ .image-shape p{background-color:rgb(0, 0, 0);padding:2px;}#chatgpt-mermaid-\_r_18p\_ .icon-shape .label rect,#chatgpt-mermaid-\_r_18p\_ .image-shape .label rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_18p\_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-\_r_18p\_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-\_r_18p\_ .node .neo-node{stroke:rgb(31, 78, 148);}#chatgpt-mermaid-\_r_18p\_ [data-look="neo"].node rect,#chatgpt-mermaid-\_r_18p\_ [data-look="neo"].cluster rect,#chatgpt-mermaid-\_r_18p\_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-\_r_18p\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_18p\_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-\_r_18p\_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-\_r_18p\_-gradient);stroke-width:1px;}#chatgpt-mermaid-\_r_18p\_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_18p\_ [data-look="neo"].node .neo-line path{stroke:rgb(31, 78, 148);filter:none;}#chatgpt-mermaid-\_r_18p\_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-\_r_18p\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_18p\_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-\_r_18p\_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-\_r_18p\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_18p\_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-\_r_18p\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_18p\_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(153, 206, 255);}#chatgpt-mermaid-\_r_18p\_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(153, 206, 255);}#chatgpt-mermaid-\_r_18p\_ .node tspan[font-weight="normal"],#chatgpt-mermaid-\_r_18p\_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-\_r_18p\_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-width:1px;}#chatgpt-mermaid-\_r_18p\_ .node rect,#chatgpt-mermaid-\_r_18p\_ .node circle,#chatgpt-mermaid-\_r_18p\_ .node ellipse,#chatgpt-mermaid-\_r_18p\_ .node polygon,#chatgpt-mermaid-\_r_18p\_ .node path{fill:rgb(0, 40, 77);stroke:rgba(255, 255, 255, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_18p\_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-\_r_18p\_ .mermaid-decision .label-container{fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-dasharray:2,2;}#chatgpt-mermaid-\_r_18p\_ .edgePaths .flowchart-link{stroke:rgb(175, 175, 175);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-\_r_18p\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_18p\_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}Speed ReferenceSpeed PI ControllerIq ReferenceD-Q Current PI ControllersId ReferenceInverse Park TransformSVPWMThree-Phase InverterPMSMPhase Current SensingClarke TransformPark TransformHall SensorsRotor Position and SpeedEstimation

**Figure 1: Closed-loop PMSM FOC architecture.**

### **Control sequence**

1. The reference motor speed is compared with the measured speed.
2. The speed PI controller generates the q-axis current reference.
3. The d-axis and q-axis current controllers regulate the corresponding current components.
4. The current controllers generate d-q voltage references.
5. Inverse Park transformation converts d-q voltages into stationary α-β components.
6. SVPWM generates three-phase inverter duty cycles.
7. The inverter supplies the required three-phase voltage to the PMSM.
8. Phase currents and rotor-position information are fed back to close the control loops.

## **3. PMSM Mathematical Modelling**

The PMSM is represented using electrical and mechanical equations in the rotor reference frame.

### **3.1 Electrical equations**

The d-axis and q-axis voltage equations are:

\\[ V_d=R_s i_d+\frac{d\psi_d}{dt}-\omega_e\psi_q \\]

\\[ V_q=R_s i_q+\frac{d\psi_q}{dt}+\omega_e\psi_d \\]

Where:

- \\(V_d,V_q\\): d-axis and q-axis voltages.
- \\(i_d,i_q\\): d-axis and q-axis currents.
- \\(R_s\\): stator resistance.
- \\(\psi_d,\psi_q\\): d-axis and q-axis flux linkages.
- \\(\omega_e\\): electrical angular velocity.

Flux linkage equations:

\\[ \psi_d=L_d i_d+\lambda_m \\]

\\[ \psi_q=L_q i_q \\]

Substituting:

\\[ V_d=R_s i_d+L_d\frac{di_d}{dt}-\omega_e L_q i_q \\]

\\[ V_q=R_s i_q+L_q\frac{di_q}{dt} +\omega_e(L_d i_d+\lambda_m) \\]

### **3.2 Electromagnetic torque**

The electromagnetic torque equation is:

\\[ T_e=\frac{3}{2}p(\psi_d i_q-\psi_q i_d) \\]

Alternatively:

\\[ T_e=\frac{3}{2}p[\lambda_m i_q+(L_d-L_q)i_d i_q] \\]

Where:

- \\(p\\): number of pole pairs.
- \\(\lambda_m\\): permanent magnet flux linkage.
- \\(L_d,L_q\\): d-axis and q-axis inductances.

For a surface-mounted PMSM, \\(L_d\\) and \\(L_q\\) are approximately equal. Therefore, torque is predominantly controlled through q-axis current.

### **3.3 Mechanical equations**

The mechanical dynamics are represented as:

\\[ J\frac{d\omega_m}{dt}=T_e-T_L-B\omega_m \\]

\\[ \frac{d\theta_m}{dt}=\omega_m \\]

Where:

- \\(J\\): moment of inertia.
- \\(T_L\\): load torque.
- \\(B\\): friction coefficient.
- \\(\omega_m\\): mechanical angular velocity.
- \\(\theta_m\\): mechanical rotor position.

Electrical and mechanical speeds are related by:

\\[ \omega_e=p\omega_m \\]

## **4. Field-Oriented Control Implementation**

FOC enables independent regulation of the motor's flux and torque components by transforming three-phase currents into the rotating d-q reference frame.

### **4.1 Clarke Transformation**

The Clarke transformation converts three-phase currents into two stationary reference-frame components.

For a balanced three-phase system:

\\[ i\_\alpha=i_a \\]

\\[ i\_\beta=\frac{1}{\sqrt{3}}(i_a+2i_b) \\]

The resulting α-β components are used for Park transformation.

### **4.2 Park Transformation**

The Park transformation converts stationary reference-frame quantities into rotating d-q components.

\\[ i_d=i\_\alpha\cos\theta_e+i\_\beta\sin\theta_e \\]

\\[ i_q=-i\_\alpha\sin\theta_e+i\_\beta\cos\theta_e \\]

Here, \\(\theta_e\\) represents the electrical rotor position.

- d-axis current controls the flux-producing component.
- q-axis current controls the torque-producing component.

### **4.3 Inverse Park Transformation**

The inverse Park transformation converts d-q voltage references back into stationary α-β components.

\\[ V\_\alpha=V_d\cos\theta_e-V_q\sin\theta_e \\]

\\[ V\_\beta=V_d\sin\theta_e+V_q\cos\theta_e \\]

These voltage references are supplied to the SVPWM subsystem.

*Note: The forward and inverse transformation equations must use consistent scaling and angle conventions throughout the implementation.*

## **5. Hall Sensor-Based Rotor Position Estimation**

Three Hall-effect sensors are used to determine rotor position.

The sensors are positioned 120° apart mechanically in the reference project and generate three digital signals.

The combination of these signals produces six valid switching states per electrical revolution, corresponding to 60° electrical sectors.

### **Hall sensor functionality**

- Rotor-position detection.
- Electrical sector identification.
- Direction-of-rotation detection.
- Rotor speed estimation from Hall transition intervals.

A typical valid Hall-state sequence is:

`001 → 101 → 100 → 110 → 010 → 011`

The actual sequence depends on motor wiring and sensor alignment.

For a 60° electrical sector:

\\[ \Delta\theta_e=60^\circ=\frac{\pi}{3} \\]

Electrical speed estimation:

\\[ \omega_e\approx\frac{\Delta\theta_e}{\Delta t} \\]

Mechanical speed:

\\[ \omega_m=\frac{\omega_e}{p} \\]

Hall sensors provide relatively coarse position information. Interpolation between Hall transitions can be implemented to improve position estimation and torque smoothness.

## **6. PI Controller Design**

The FOC architecture uses a cascaded control structure.

### **6.1 Outer Speed Control Loop**

The speed error is:

\\[ e\_\omega=\omega^\*-\omega_m \\]

The PI controller generates the q-axis current reference:

\\[ i_q^\*=K\_{p\omega}e\_\omega+ K\_{i\omega}\int e\_\omega dt \\]

The q-axis current reference determines the torque demand.

### **6.2 Inner d-axis Current Controller**

For a surface-mounted PMSM operating below base speed, the d-axis reference is generally:

\\[ i_d^\*=0 \\]

The current error is:

\\[ e_d=i_d^\*-i_d \\]

The PI controller generates the d-axis voltage reference:

\\[ V_d^\*=K\_{pd}e_d+K\_{id}\int e_d dt \\]

### **6.3 Inner q-axis Current Controller**

The q-axis current error is:

\\[ e_q=i_q^\*-i_q \\]

The PI controller generates the q-axis voltage reference:

\\[ V_q^\*=K\_{pq}e_q+K\_{iq}\int e_q dt \\]

### **Controller tuning procedure**

1. Initialize motor electrical and mechanical parameters.
2. Tune the d-axis and q-axis current controllers.
3. Verify current tracking and stability.
4. Tune the outer speed controller.
5. Introduce current and voltage saturation limits.
6. Implement anti-windup to prevent integrator accumulation during saturation.
7. Evaluate performance under speed and load disturbances.

The current loops should be significantly faster than the outer speed loop.

## **7. Space Vector Pulse Width Modulation (SVPWM)**

SVPWM is used to generate the switching signals for the three-phase inverter.

The d-q voltage references are first converted into stationary α-β voltage references using inverse Park transformation.

The SVPWM algorithm then calculates the required inverter switching duty cycles.

### **Main steps**

1. Calculate the reference voltage vector.
2. Identify the corresponding SVPWM sector.
3. Calculate active-vector dwell times.
4. Calculate zero-vector dwell time.
5. Determine the three-phase duty cycles.
6. Generate PWM signals for the inverter switches.

SVPWM provides efficient utilization of the DC-link voltage and enables controlled three-phase voltage generation.

The inverter implementation must also account for dead time, complementary gate signals and protection against shoot-through.

## **8. MATLAB/Simulink Project Structure**

The project is organized into modular MATLAB functions and documentation.

```
PMSM_FOC_Project/
│
├── README.md
│
├── docs/
│   ├── PMSM_FOC_Theory_and_Implementation.md
│   └── Simulink_Model_Build_Guide.md
│
├── scripts/
│   ├── motor_parameters.m
│   ├── hall_sensor_model.m
│   ├── coordinate_transforms.m
│   ├── pi_controller.m
│   └── svpwm.m
│
└── models/
    └── PMSM_FOC_Hall_Model.slx
```

*The main Simulink model is to be constructed and validated; it is not included as a prebuilt model in the reference repository.*

### **MATLAB modules**

| MATLAB file               | Functionality                                       |
| ------------------------- | --------------------------------------------------- |
| `motor_parameters.m`      | Motor and controller parameter initialization       |
| `hall_sensor_model.m`     | Hall sensor modelling and rotor-position estimation |
| `coordinate_transforms.m` | Clarke, Park and inverse transformations            |
| `pi_controller.m`         | PI controller implementation                        |
| `svpwm.m`                 | SVPWM duty-cycle generation                         |

## **9. Motor Parameters**

The reference project provides the following illustrative motor parameters:

| Parameter                     | Value         |
| ----------------------------- | ------------- |
| Stator resistance             | 0.285 Ω       |
| Simplified stator inductance  | 0.85 mH       |
| Permanent magnet flux linkage | 0.0118 Wb     |
| Number of pole pairs          | 4             |
| Moment of inertia             | 0.00001 kg·m² |
| Friction coefficient          | 0.00001 N·m·s |

These values should be replaced with actual motor parameters before validating the controller.

## **10. Simulation Test Cases**

The following test cases are proposed for validating the complete Simulink model.

**Test 1: No-load startup**

- Start the motor from zero speed.
- Apply the reference speed command.
- Observe acceleration and speed tracking.
- Evaluate current and torque responses.

**Test 2: Speed reference variation**

- Apply a step change in reference speed.
- Observe transient response.
- Evaluate overshoot, rise time and settling time.

**Test 3: Load torque disturbance**

- Operate the motor at steady-state speed.
- Introduce a sudden increase in load torque.
- Observe speed deviation and q-axis current response.
- Evaluate disturbance rejection.

**Test 4: Current regulation**

- Monitor d-axis and q-axis currents.
- Verify that d-axis current tracks its reference.
- Verify that q-axis current follows the torque command.

**Test 5: Hall sensor validation**

- Monitor all three Hall signals.
- Verify six valid states per electrical revolution.
- Evaluate rotor-position and speed estimation.

**Test 6: SVPWM validation**

- Observe three-phase duty-cycle waveforms.
- Verify switching sequence and duty-cycle limits.
- Check inverter gate-signal interlocking.

## **11. Performance Evaluation**

The reference repository describes the following achievable performance targets with suitable motor parameters and controller tuning.

| Performance parameter  | Reference target |
| ---------------------- | ---------------- |
| Speed tracking error   | Less than 1%     |
| Rise time              | 100–200 ms       |
| Settling time          | 200–300 ms       |
| Overshoot              | Less than 5%     |
| Current-loop bandwidth | 1–2 kHz          |
| Speed-loop bandwidth   | 50–200 Hz        |

These are reference targets, not experimentally verified results. Actual values must be obtained from the completed simulation.

### **Recommended simulation waveforms**

[Tune Field-Oriented Controllers Using SYSTUNE - MATLAB & Simulink](https://images.openai.com/static-rsc-4/wkOcvOUz6E0wzg-n0vlpDw-cZlkCJxlylYQnJAO8jeYlyl0EIyoQlGXUMk6Z6lSYM-IbTOiZJlyoFZaqsbSFgkpqGETK1g_lcnElwW4fMqCzCXI7Q0nKXnjipeJHX61X6xXsyLzS0AdVgiaTTPm0q9GnEz1VN098WRiHbG1oGmc?purpose=inline)

**Speed response**

[Import PMSM Flux Linkage Data from JMAG-RT - MATLAB & Simulink](https://images.openai.com/static-rsc-4/gu8HL6_UVVloKU52j6TnFfEcqu5wSP0e9XNVqrnoCLVTEpceSoUU00JAVXQJ5kK65gph_VtI4TD7-5FrGM7hvpfpO2ZIuRHB89Lvm7kACo25a4MMPUouBkq9F1fKEMf8_6eCRvWV-rafUSgm7efaLRFcoT3r0SmeImsfAWaC_Lc?purpose=inline)

**Three-phase stator currents**

[Control and Simulate Torque of IPMSM in DQ Frame - MATLAB & Simulink](https://images.openai.com/static-rsc-4/Av36oAJFcktf8DeZ6Gd1VKSQyKbS7G1uA8sOStvakaCGubIInHEcBmvB2uAtnWswMJg94cbJoatYrSEnJ4tVgpGeQCeTJm8MTJ3BFQ7Q3SU1y6mClMOjeVF6FDpdvUF-If3JkRmwE4_saIU7k4OO4yYlGxfI0_C33_3qcPj6Txg?purpose=inline)

**d-q current response**

[SVPWM Waveforms Clarification - hardware support - SimpleFOC Community](https://images.openai.com/static-rsc-4/k6KhJYP7MFW-xQ466MXUoRBCAgqhD4LguDj1zizD5Gxqc-6BFrVoHkq1QyUu2vicezZrY7lO97UkYIcLEVtx6Jd16WHtsHq_N_cEnfwv9NF1yetyhKKrDzFoy1mZZhU3SOnKU1FZL7oUJCcdloRZSiUgrokvYP_Pi9BQVgnZQW0?purpose=inline)

**SVPWM waveforms**

*These are illustrative waveform references, not results generated by this project.*

## **12. Troubleshooting**

| Problem                | Possible reason                               |
| ---------------------- | --------------------------------------------- |
| Motor does not start   | Incorrect Hall sequence or phase mapping      |
| Speed oscillations     | Improper speed PI tuning                      |
| Current oscillations   | Excessive current PI gains                    |
| Steady-state error     | Insufficient integral action                  |
| High torque ripple     | Rotor-position estimation errors              |
| Duty-cycle saturation  | DC-link voltage limitation                    |
| Simulation instability | Incorrect motor parameters or solver settings |

## **13. Future Scope**

The following enhancements can extend the project:

- Sensorless PMSM FOC using back-EMF observers.
- Sliding Mode Observer (SMO) for rotor-position estimation.
- Field weakening for high-speed operation.
- Maximum Torque Per Ampere (MTPA) control.
- Dead-time compensation.
- ADC synchronization with PWM switching.
- Embedded implementation using NXP S32K or TI C2000 microcontrollers.
- Hardware validation of current sensing, PWM generation and motor-control performance.

## **14. Relevance to Embedded Motor Control**

This project provides a foundation for understanding embedded motor-control algorithms and their interaction with microcontroller peripherals.

Key embedded concepts include:

- ADC-based phase-current and DC-link voltage measurement.
- PWM timer configuration and duty-cycle updates.
- Hall sensor digital-input capture.
- Real-time execution of Clarke and Park transformations.
- PI-based current and speed regulation.
- SVPWM generation.
- Overcurrent and overvoltage protection.

The simulation-level implementation can be extended to embedded hardware for real-time motor-control validation.

## **15. References**

1. NXP Semiconductors – [Motor Control Resources](https://www.nxp.com/design/design-center/software/motor-control-software:MOTOR-CONTROL-SOFTWARE?utm_source=chatgpt.com).
2. MathWorks – [PMSM Field-Oriented Control](https://www.mathworks.com/help/mcb/?utm_source=chatgpt.com).
3. Microchip – [Sensorless Field-Oriented Control of PMSM (AN1078)](https://ww1.microchip.com/downloads/en/AppNotes/01078B.pdf?utm_source=chatgpt.com).
4. R. Krishnan – *Permanent Magnet Synchronous and Brushless DC Motor Drives*.
5. D. W. Novotny and T. A. Lipo – *Vector Control and Dynamics of AC Drives*.

## **16. Conclusion**

This project demonstrates the design methodology of a closed-loop PMSM drive using Field-Oriented Control. By combining mathematical motor modelling, coordinate transformations, cascaded PI controllers, Hall sensor feedback and SVPWM, the system provides a structured approach to understanding modern electric motor drives.

The project also establishes a foundation for embedded motor-control development, particularly in ADC interfacing, PWM generation, real-time control algorithms and microcontroller-based motor-drive applications.


