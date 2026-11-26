# Pragatisheel Smart Energy Ecosystem for Nepal (PSEEN) & PSO-PID Control

[![Energy Hackathon](https://img.shields.io/badge/Energy%20Hackathon-Team%20Power%20Pulse-0b2545)](https://github.com/Rajubahardar/Energy-hackthaon)
[![Community BESS](https://img.shields.io/badge/Community%20BESS-100%E2%80%93500%20kW%20%7C%20200%E2%80%931000%20kWh-2E7D32)](#2-master-system--dispatch-parameters)
[![Optimizer](https://img.shields.io/badge/Control%20Optimizer-PSO--Tuned%20PID%20(ISE%2FIAE%2FITSE%2FITAE)-6A1B9A)](#3-intelligent-control-layer-pso-tuned-pid-optimization)
[![Grid Context](https://img.shields.io/badge/Grid%20Standard-Nepal%20NEA%20%7C%20Hybrid%20Hydro--Solar--Wind-1d4e89)](#1-project-overview--nepal-grid-problem-statement)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](#license)

An integrated smart-grid supervisory dispatch and intelligent control framework developed by **Team Power Pulse (Ingoude Technologies)** for the **Energy Hackathon**. The project addresses Nepal's seasonal run-of-river hydropower imbalance, evening peak grid strain, and power-quality instability by combining a **Smart Energy Management System (SEMS)** with **Community Battery Energy Storage Systems (BESS)** and **Particle Swarm Optimization (PSO)-tuned PID control**.

> **Presentation Decks & Documentation:** Review the grid architecture deck **[`Pragatisheel Smart Energy Ecosystem for Nepal.pptx`](Pragatisheel%20Smart%20Energy%20Ecosystem%20for%20Nepal.pptx)** and the final-round control simulation deck **[`Presentationfinal (1).pptx`](Presentationfinal%20(1).pptx)** directly in this repository.

---

## Quick Local Recovery (2-Line PowerShell Command)

Restore the complete repository directly to your Windows Desktop anytime using PowerShell:

```powershell
cd "$env:USERPROFILE\Desktop"
git clone [https://github.com/Rajubahardar/Energy-hackthaon.git](https://github.com/Rajubahardar/Energy-hackthaon.git)
```

> **No Git installed?** Click the green **Code** button at the top of this repository and select **Download ZIP**.

---

## Key Highlights

- **Dual-Topology Smart Energy Management (`PSEEN`):** Coordinates peer-to-peer solar/wind microgrid sharing across rural hilly communities and time-of-use peak shaving (`06:00 PM – 11:00 PM`) for urban feeders through a centralized **Smart Energy Management System (SEMS)**.
- **Utility-Scale Community Battery Storage (`100–500 kW` / `200–1,000 kWh`):** Sized at `20×` to `50×` residential battery capacity (`$250,000–$5,000,000` capital range) to absorb off-peak (`11:00 PM – 06:00 AM`) and daytime (`06:00 AM – 06:00 PM`) surplus energy for evening peak dispatch.
- **Model-Free PSO-Tuned PID Control (`PSO-PID1`–`PSO-PID4`):** Optimizes controller gains $(K_p, K_i, K_d)$ across `ISE`, `IAE`, `ITSE`, and `ITAE` error criteria, eliminating the severe overshoot (`~1.68 pu` peak) and oscillations observed under classical **Ziegler-Nichols (`ZN-PID`)** tuning.
- **Transparent Engineering & Validation Boundaries:** Explicitly distinguishes conceptual multi-mode SEMS dispatch logic, step-response PID simulation benchmarks, and future physical/hardware-in-the-loop validation phases.

---

## 1. Project Overview & Nepal Grid Problem Statement

Nepal's electricity network relies heavily on run-of-river (RoR) hydropower plants, creating sharp seasonal and diurnal supply-demand mismatches:

1. **Seasonal Generation Imbalance (Run-of-River Dependency):** Monsoon river flows produce substantial wet-season surpluses (with Nepal exporting electricity worth **Rs. 12.72 billion** to India during the first four months of FY `2080/81`), whereas dry-season flow drops create severe winter deficits (such as NEA requesting **600 MW** of imports—`450 MW` from Bihar and `150 MW` from Uttar Pradesh—with **350 MW** approved).
2. **Evening Peak-Hour Grid Strain (`06:00 PM – 11:00 PM`):** Concentrated evening residential and commercial demand places maximum stress on generation and transmission assets.
3. **Power Quality & Asset Degradation:** Voltage and frequency deviations, sluggish transient recovery, and uncoordinated battery charging cause:
   - Power quality degradation across distribution feeders
   - Accelerated equipment wear and higher maintenance costs
   - Reduced system efficiency and renewable curtailment/waste
   - Reliability issues and unplanned power interruptions
   - Premature battery capacity degradation

To resolve both macro-level energy balancing and micro-level dynamic stability, the architecture operates across two coordinated layers:

- **Supervisory Dispatch Layer (`PSEEN` / `SEMS`):** Manages energy routing between solar/wind microgrids, community battery storage, local loads, and the Nepal Electricity Authority (NEA) distribution line across Rural and Urban operating models.
- **Dynamic Control Layer (`PSO-PID`):** Tunes proportional-integral-derivative controllers via Particle Swarm Optimization to stabilize frequency, voltage, turbine-governor response, and battery charging dynamics.

---

## 2. Master System & Dispatch Parameters

All system ratings, operating windows, and storage specifications documented across the architecture decks are consolidated below:

| Parameter | Specification | Units / Scale | Engineering Notes |
| :--- | :---: | :---: | :--- |
| **Community BESS Power Rating** | `100 – 500` | $\text{kW}$ | Utility-scale community storage unit |
| **Community BESS Energy Capacity** | `200 – 1,000` | $\text{kWh}$ | Equivalent to `20×` to `50×` residential home battery size |
| **Estimated BESS Capital Cost** | `$250,000 – $5,000,000` | $\text{USD}$ | Scales with power rating, energy capacity, and inverter hardware |
| **Rural Generation Mix** | `Solar + Wind` | $\text{Hybrid Microgrids}$ | Community A (Solar Microgrid) & Community B (Wind Microgrid) |
| **Urban Off-Peak Charging Window** | `11:00 PM – 06:00 AM` | $\text{7 hours}$ | Community BESS charges from NEA's grid line via SEMS |
| **Urban Daytime Charging Window** | `06:00 AM – 06:00 PM` | $\text{12 hours}$ | Community BESS charges from local solar panels via SEMS |
| **Urban Peak Dispatch Window** | `06:00 PM – 11:00 PM` | $\text{5 hours}$ | Community BESS supplies peak load with NEA grid backup/export |
| **Tuned Controller Parameters** | $(K_p, K_i, K_d)$ | $\text{Dimensionless / s}$ | Proportional, Integral, and Derivative gains optimized via PSO |
| **Optimization Fitness Criteria** | `ISE, IAE, ITSE, ITAE` | $\text{Error Integrals}$ | Time-weighted and squared transient error objective functions |

---

## 3. System Architecture & Operating Models

### A. Rural / Hilly Area Model (Peer-to-Peer Community Microgrid Sharing)

Designed for geographically separated hilly communities equipped with complementary renewable microgrids (**Community A** powered by Solar + BESS; **Community B** powered by Wind + BESS) coordinated by **SEMS**:

| Operating Condition | SEMS Dispatch Decision | Energy Flow Path |
| :--- | :--- | :--- |
| **1. Balanced Generation** | Local self-supply & storage charging | Solar/Wind microgrids supply local loads and charge Community A & B batteries. |
| **2. Community A Deficit** | Inter-community peer support | SEMS routes surplus energy from **Community B** to **Community A** before touching the main grid. |
| **3. Combined Local Deficit** | Grid import backup | When Community B cannot cover Community A's shortfall, SEMS imports the deficit from **NEA's grid line**. |
| **4. Dual-Community Surplus** | Grid export feed-in | When both Community A and Community B hold surplus energy, SEMS exports excess power to **NEA's grid line**. |

---

### B. Urban Model (Time-of-Use Peak Shaving & Grid Support)

Designed for urban distribution feeders experiencing sharp evening peak demand (`06:00 PM – 11:00 PM`):

| Time Window | Grid / Storage State | SEMS Dispatch Action |
| :--- | :--- | :--- |
| **`11:00 PM – 06:00 AM`** | **Off-Peak Hours** | Charges Community Battery Storage from **NEA's grid line** during low-demand periods. |
| **`06:00 AM – 06:00 PM`** | **Normal / Daytime Hours** | Charges Community Battery Storage from local **Solar Panels**. |
| **`06:00 PM – 11:00 PM`** | **Peak Hours (Sufficient BESS)** | Discharges Community Battery Storage via SEMS to supply urban peak demand independently. |
| **`06:00 PM – 11:00 PM`** | **Peak Hours (Insufficient BESS)** | Combines Community Battery discharge with supplemental power from **NEA's grid line**. |
| **`06:00 PM – 11:00 PM`** | **Peak Hours (Surplus BESS)** | Supplies local peak load and exports surplus battery power back into **NEA's grid line**. |

---

## 4. Intelligent Control Layer: PSO-Tuned PID Optimization

Classical heuristic tuning methods such as **Ziegler-Nichols (`ZN-PID`)** push the plant to sustained oscillation to find ultimate gain, often producing high overshoot and poor damping on non-linear hybrid power systems. **Particle Swarm Optimization (PSO)** treats controller tuning as a model-free multi-dimensional search across $(K_p, K_i, K_d)$:

### PSO-PID Optimization Loop
1. **Swarm Initialization:** Initialize particles with candidate gain vectors $(K_p, K_i, K_d)$ and velocities $v$.
2. **Closed-Loop Fitness Evaluation:** Simulate system step response and compute error performance (`ISE`, `IAE`, `ITSE`, or `ITAE`).
3. **Best-Position Tracking:** Update each particle's personal best (`pbest`) and the swarm's global best (`gbest`).
4. **Velocity & Position Update:** Adjust particle trajectories toward `pbest` and `gbest` until convergence criteria are met.

### Step-Response Simulation Comparison (`ZN-PID` vs. `PSO-PID1`–`PSO-PID4`)

Comparative unit-step response simulations evaluate classical `ZN-PID` against four PSO-tuned controller variants (`PSO-PID1` through `PSO-PID4`) over a `10 s` horizon:

| Controller Variant | Transient Overshoot Behavior | Settling & Damping Characteristics |
| :--- | :--- | :--- |
| **`ZN-PID` (Baseline)** | High initial peak overshoot (`~1.68 pu` on unit step) | Pronounced underdamped oscillations requiring `~5–6 s` to settle at `1.0 pu`. |
| **`PSO-PID1` – `PSO-PID4`** | Near-zero to minimal overshoot (`~1.00–1.10 pu`) | Fast, smooth rise and monotonic/critically damped settling within `~2–4 s`. |

### Target Engineering Applications
- **Load Frequency Control (LFC):** Maintaining nominal grid frequency under fluctuating renewable generation and load steps.
- **Hydropower Turbine & Generator Control:** Regulating governor speed and terminal voltage in RoR/storage hydro plants.
- **Battery Charging Control:** Ensuring safe, efficient constant-current/constant-voltage BESS charging profiles.
- **Electric Vehicles, Pumping & Robotics:** Stabilizing EV motor/battery bus voltage, irrigation pumping flow, and aerospace/robotic actuators.

---

## 5. Validation Scope & Engineering Boundaries

> [!NOTE]
> **Engineering Transparency Disclosure:** This repository archives the complete Energy Hackathon system design and control simulation deliverables. Validation boundaries are categorized explicitly below:

| Validation Tier | Status | Technical Scope |
| :--- | :---: | :--- |
| **1. SEMS Rural & Urban Dispatch Logic** | **DESIGN-LEVEL COMPLETE** | Multi-state energy routing rules (P2P microgrid sharing, ToU charging, and NEA grid import/export states) formalized in `Pragatisheel Smart Energy Ecosystem for Nepal.pptx`. |
| **2. PSO-PID Step-Response Simulation** | **SIMULATION-VERIFIED** | Closed-loop unit-step benchmarking comparing `ZN-PID` against `PSO-PID1`–`PSO-PID4` over a `10 s` window in `Presentationfinal (1).pptx`. |
| **3. Multi-Bus NEA Feeder Power Flow** | **DEFERRED / FUTURE WORK** | Full 3-phase distribution load flow, inverter PWM switching dynamics, and LCL filter resonance modeling. |
| **4. Physical Hardware & Tariff Economics** | **OUT OF SCOPE (PROTOTYPE)** | Site-specific NEA Time-of-Use tariff optimization, BESS cycle-life degradation modeling, and physical SCADA/HIL deployment. |

---

## 6. Repository Structure

```text
Energy-hackthaon/
├── Pragatisheel Smart Energy Ecosystem for Nepal.pptx   # Round 1 deck: Nepal grid challenges, Rural/Urban SEMS & BESS sizing
├── Presentationfinal (1).pptx                           # Final deck: PSO-PID control theory, flowchart & step-response results
└── README.md                                            # Project architecture, recovery commands & technical documentation
```

*(If additional `.m` or `.slx` simulation scripts were included in your local push, they appear alongside the presentation decks in the root directory.)*

---

## 7. Getting Started After Recovery

1. **Recover the repository to your Desktop:**
   ```powershell
   cd "$env:USERPROFILE\Desktop"
   git clone [https://github.com/Rajubahardar/Energy-hackthaon.git](https://github.com/Rajubahardar/Energy-hackthaon.git)
   cd "$env:USERPROFILE\Desktop\Energy-hackthaon"
   ```
2. **Inspect the Architecture & Simulation Decks:**
   - Open `Pragatisheel Smart Energy Ecosystem for Nepal.pptx` to review the Rural/Urban SEMS state diagrams and community BESS sizing.
   - Open `Presentationfinal (1).pptx` to review the PSO-PID tuning algorithm flowchart and `ZN-PID` vs. `PSO-PID` step-response curves.
3. **Run MATLAB / Simulink Models (if present locally):**
   - Launch MATLAB (`R2023b` or later), navigate to the cloned folder, and execute any included `.m` or `.slx` scripts to reproduce the step-response curves.

---

## Team — Power Pulse (Ingoude Technologies)

- **Aayushman Shrestha**
- **Raju Bahardar** — [@Rajubahardar](https://github.com/Rajubahardar)
- **Sanjaya Limbu**
- **Sumit Karki**

---

## Citation

```bibtex
@misc{powerpulse2026pseen,
  author       = {Shrestha, Aayushman and Bahardar, Raju and Limbu, Sanjaya and Karki, Sumit},
  title        = {Pragatisheel Smart Energy Ecosystem for Nepal (PSEEN) and PSO-Tuned PID Control},
  year         = {2026},
  howpublished = {\url{[https://github.com/Rajubahardar/Energy-hackthaon](https://github.com/Rajubahardar/Energy-hackthaon)}},
  note         = {Energy Hackathon Submission — Team Power Pulse (Ingoude Technologies)}
}
```

---

## License

Distributed under the **MIT License**.
