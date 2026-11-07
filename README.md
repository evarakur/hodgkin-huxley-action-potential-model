# Hodgkin–Huxley Action Potential Model

A Python computational neuroscience project simulating neuronal action potentials with the Hodgkin–Huxley model. The notebook explores how voltage-dependent ion-channel kinetics generate electrical spikes, how stimulus amplitude affects firing, and how numerical integration choices influence simulation reliability.

## Project Overview

The Hodgkin–Huxley model represents a neuron as four coupled ordinary differential equations for membrane voltage (`V`) and sodium activation (`m`), sodium inactivation (`h`), and potassium activation (`n`). This project implements the model, simulates responses to brief current pulses, and investigates the effects of numerical methods and physiological parameters.

## Mathematical Model

The membrane voltage equation is

$$
C\_m\frac{dV}{dt}=I\_{\mathrm{ext}}-I\_{\mathrm{Na}}-I\_{\mathrm{K}}-I\_{\mathrm{L}},
$$

with ionic currents

$$
I\_{\mathrm{Na}}=G\_{\mathrm{Na}}m^{3}h(V-E\_{\mathrm{Na}}),\qquad
I\_{\\mathrm{K}}=G\_{\mathrm{K}}n^{4}(V-E\_{\mathrm{K}}),\qquad
I\_{\mathrm{L}}=G\_{\mathrm{L}}(V-E\_{\mathrm{L}}).
$$

Each gating variable $q \in \lbrace m,h,n \rbrace$ follows voltage-dependent activation and inactivation kinetics:

$$
\frac{dq}{dt}=\alpha\_q(V)(1-q)-\beta\_q(V)q.
$$

The notebook uses the classic squid giant axon parameterization, with a nominal resting voltage of −65 mV.

## Methods and Analyses

1. **Action potential simulation:** Apply a 20 µA/cm² current pulse lasting 0.5 ms and visualize membrane depolarization, repolarization, and afterhyperpolarization.
2. **Ion-channel dynamics:** Plot the sodium activation/inactivation and potassium activation variables, and compare membrane voltage with the instantaneous conductance-weighted reversal potential.
3. **Firing threshold and all-or-none behavior:** Sweep stimulus amplitudes, refine the threshold by binary search, and compare closely spaced subthreshold and suprathreshold stimuli.
4. **Numerical integration:** Implement forward Euler and explicit midpoint methods, then compare their results and behavior at coarse time steps.
5. **Numerical stability:** Examine the effective membrane time constant as conductances change during a spike.
6. **Sodium activation sensitivity:** Slow sodium activation by scaling its time constant and estimate when the stimulus no longer produces a spike under a specified voltage criterion.
7. **Resting-potential calibration:** Derive the leak reversal potential needed for equilibrium at −65 mV and simulate changes after shifting this parameter.

## Selected Results

The following values are from the saved notebook outputs.

| Analysis | Result |
| --- | --- |
| Action potential, 20 µA/cm² stimulus | Peak **+39.6 mV**; undershoot **−76.2 mV** |
| Estimated firing threshold | Approximately **13.239 µA/cm²** for a 0.5 ms pulse and peak-voltage criterion of +15 mV |
| Near-threshold comparison | **13.2356** versus **13.2621 µA/cm²** produces subthreshold versus spiking responses |
| Euler versus midpoint, fine time step | Peak voltages **+39.352 mV** versus **+39.326 mV** |
| Coarse-step instability (`dt = 0.1 ms`) | First non-finite voltage at **8.0 ms** (Euler) and **7.4 ms** (midpoint) |
| Effective membrane time constant | Approximately **1.477 ms** at rest and **0.0280 ms** at maximum conductance |
| Sodium activation slowdown | Spike criterion ceases to be met near **k = 2.0553** |
| Leak reversal potential for −65 mV equilibrium | Calculated **−54.4011 mV**, compared with the model value **−54.3870 mV** |
| Shifted leak reversal potential | Final voltages **−66.436 mV** (−5 mV shift) and **−63.828 mV** (+5 mV shift) |

These results are specific to the model parameters, stimulus protocol, integration settings, and spike-detection criterion used in the notebook.

## Tools and Techniques

- **Python**, **NumPy**, **Matplotlib**, and **Jupyter Notebook**
- Coupled ordinary differential equations and numerical simulation
- Forward Euler and explicit midpoint integration
- Parameter sweeps, binary search, sensitivity analysis, and model calibration
- Time-series visualization and numerical stability analysis

## Run the Notebook

1. Clone or download this repository.
2. Install the required packages:

   ```bash
   pip install numpy matplotlib notebook
   ```

3. Launch Jupyter Notebook:

   ```bash
   jupyter notebook
   ```

4. Open [`Hodgkin_Huxley_Action_Potential_Model.ipynb`](Hodgkin_Huxley_Action_Potential_Model.ipynb) and select **Run All** to reproduce the analyses and figures.

The notebook can also be opened in Google Colab.

## Limitations

- This is a classical single-compartment conductance-based model, not a spatially detailed neuron model or a fit to newly collected experimental data.
- The threshold estimate depends on the chosen stimulus waveform and operational spike criterion.
- Explicit integration methods can become unstable at large time steps; a higher-order method is not automatically stable.
- The membrane time-constant estimate provides intuition about fast dynamics but is not a complete stability analysis of the nonlinear four-variable system.
- The nominal resting voltage is part of the model's parameterization rather than an independent prediction.

## Project Context

Adapted and documented as a computational biology portfolio project from coursework in biological dynamical systems. The classic Hodgkin–Huxley model and course-provided starting definitions are distinguished from the notebook's numerical experiments and analysis. Confirm permission before publicly redistributing any instructor-provided material retained in the notebook.
