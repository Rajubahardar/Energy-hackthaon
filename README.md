# ⚡ Pragatisheel Smart Energy Ecosystem for Nepal (PSEEN) & PSO-PID Control

**Energy Hackathon Project — Team Power Pulse (Ingoude Technologies)**

An integrated smart-grid and intelligent-control solution designed to address Nepal's seasonal hydropower fluctuations, evening peak grid strain, and power-quality instability through **community energy storage (SEMS)** and **Particle Swarm Optimization (PSO)-tuned PID control**.

---

## 🚀 Quick Local Recovery

Restore the complete repository directly to your Windows Desktop using these two lines in **PowerShell**:

```powershell
cd "$env:USERPROFILE\Desktop"
git clone [https://github.com/Rajubahardar/Energy-hackthaon.git](https://github.com/Rajubahardar/Energy-hackthaon.git)
```

> **No Git installed?** Click the green **Code** button above and select **Download ZIP**.

---

## 📌 Problem Statement

Nepal's power grid faces three core operational challenges:

1. **Seasonal Generation Imbalance (Run-of-River Dependency):** Heavy monsoon flows create wet-season electricity surpluses (e.g., Rs. 12.72 billion exported to India in the first four months of FY 2080/81), while dry-season river drops force winter power imports (e.g., NEA requesting 600 MW from India with 350 MW approved).
2. **Peak-Hour Grid Strain (06:00 PM – 11:00 PM):** Evening demand spikes place maximum stress on generation and transmission infrastructure.
3. **Power Quality & Asset Degradation:** Voltage/frequency deviations, slow transient response, and unoptimized charging cycles increase equipment wear, energy waste, and battery degradation.

---

## 🏗️ Solution Architecture

### 1. Pragatisheel Smart Energy Ecosystem for Nepal (PSEEN)
A **Smart Energy Management System (SEMS)** paired with **Community Battery Storage** (`100 kW / 200 kWh` to `500 kW / 1,000 kWh`) deployed across two operating models:

* **Rural / Hilly Area Model (Peer-to-Peer Microgrid Sharing):**
  * **Community A** (Solar Microgrid + Battery Storage) and **Community B** (Wind Microgrid + Battery Storage) coordinate through **SEMS**.
  * If one community runs low, SEMS routes surplus energy from the neighboring community first, draws from **NEA's grid line** only if combined local storage is insufficient, and exports excess energy back to NEA when both communities have a surplus.
* **Urban Model (Time-of-Use Peak Shaving):**
  * **Off-Peak Hours (`11:00 PM – 06:00 AM`):** Charges community battery storage from NEA's grid.
  * **Normal Hours (`06:00 AM – 06:00 PM`):** Charges community battery storage via local solar panels.
  * **Peak Hours (`06:00 PM – 11:00 PM`):** Dispatches stored battery energy to local consumers via SEMS, supplementing from NEA only when demand exceeds battery capacity or exporting surplus power back to the grid.

### 2. Intelligent Control Layer — PID Tuning with Particle Swarm Optimization (PSO)
To maintain frequency, voltage, and battery-charging stability across hybrid hydro-solar-wind networks, classical Ziegler-Nichols (`ZN-PID`) heuristic tuning is replaced with **PSO-based PID optimization**:

* **Parameter Optimization:** Searches optimal proportional, integral, and derivative gains $(K_p, K_i, K_d)$ without requiring an exact linear plant model.
* **Fitness Functions:** Minimizes standard control error criteria (`ISE`, `IAE`, `ITSE`, `ITAE`).
* **Validated Results:** Comparative step-response simulations (`ZN-PID` vs. `PSO-PID1`–`PSO-PID4`) demonstrate significantly lower overshoot, faster settling time, and smoother stability—directly applicable to **Load Frequency Control (LFC)**, **hydropower turbine/generator governing**, and **battery charging control**.

---

## 🛠️ Tech Stack & Repository Contents

* **Control & Optimization:** PID Control, Particle Swarm Optimization (PSO), Ziegler-Nichols Benchmarking
* **Simulation & Analysis:** MATLAB / Simulink (step-response modeling & error-index evaluation)
* **Grid Architecture:** Smart Energy Management System (SEMS), Hybrid Solar/Wind Microgrids, Community BESS
* **Presentation Decks Included:**
  * `Pragatisheel Smart Energy Ecosystem for Nepal.pptx` — Grid problem analysis, Rural & Urban SEMS models, and community battery sizing
  * `Presentationfinal (1).pptx` — Final-round PSO-PID control theory, flowchart, applications, and step-response simulation comparison

---

## ⚙️ Getting Started (After Recovery)

1. Navigate into the cloned directory:
   ```powershell
   cd "$env:USERPROFILE\Desktop\Energy-hackthaon"
   ```
2. Open the presentation decks or load any simulation scripts in **MATLAB / Simulink** to inspect and run the `ZN-PID` vs. `PSO-PID` step-response comparisons.

---

## 👥 Team — Power Pulse (Ingoude Technologies)

* **Aayushman Shrestha**
* **Raju Bahardar** ([@Rajubahardar](https://github.com/Rajubahardar))
* **Sanjaya Limbu**
* **Sumit Karki**
