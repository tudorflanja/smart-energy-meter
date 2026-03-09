# Smart Energy Meter — Virtual Instrument for Household Power Analysis

This project implements an interactive **web-based virtual instrument** for analyzing household electricity consumption.  
It allows users to **add and simulate electrical devices**, visualize their **daily load curve**, estimate **monthly cost**, **CO₂ footprint**, and compute an **approximate power factor (PF)** — all in real time.

Built entirely in **HTML, CSS, and JavaScript (OOP)**, the project runs directly in the browser without any backend dependencies.

---

## Overview

The Smart Energy Meter helps users understand and optimize their energy usage by:
- Simulating household devices and their daily operation schedules
- Displaying total active power for each hour of the day
- Estimating total energy consumption, monthly costs, and CO₂ emissions
- Visualizing data through an interactive **load curve** and **energy distribution pie chart**

---

## Core Features

- **Real-time energy metrics**
  - Active power, daily and monthly energy
  - Cost estimation and carbon footprint
  - Power factor (PF) approximation based on device reactive load

- **OOP Architecture**
  - `Dispozitiv` class — models an electrical appliance  
  - `ContorInteligent` class — aggregates all devices and performs calculations  
  - `afiseazaKPI()` method — dedicated for displaying current values (explicit per project requirement)

- **Dynamic Visualizations (HTML5 Canvas)**
  - **Load curve** — total hourly active power for a 24h cycle  
  - **Energy pie chart** — share of each device in total monthly energy

- **User Interaction**
  - Editable device table (add, modify, delete)
  - Built-in presets (fridge, TV, laptop, air conditioner, washing machine, lights)
  - Adjustable tariff, number of days, and CO₂ factor

---

## How It Works

Each device is defined by:
- Nominal power `P` (W)
- Power factor `PF`
- Quantity `Nr`
- Two optional operating intervals (`ON1–Dur1`, `ON2–Dur2`)
- Standby power when idle

The total hourly power is computed as:

$$
P_{total}(h) = \sum_i N_i \times [ P_{i,active}(h) + P_{i,standby}(h) ]
$$

Reactive power is estimated from:

$$
Q_i = P_i \times \tan(\arccos(PF_i))
$$

Then:

$$
PF_{total} \approx \frac{\sum P_i}{\sum \sqrt{P_i^2 + Q_i^2}}
$$

---

## Example Output

| Metric | Example Value |
|:--|--:|
| Average Power | 136 W |
| Energy/day | 3.26 kWh |
| Energy/month | 97.8 kWh |
| Cost/month | 83.1 lei |
| PF (approx.) | 0.932 |
| CO₂/month | 24.4 kg |

---

## OOP Structure

### Class `Dispozitiv`
Represents a household device with properties:
```js
class Dispozitiv {
  constructor({name, P, pf, qty, on1, dur1, on2, dur2, standby}) { ... }
  P_ora(h)        // active power at hour h
  Q_ora(h)        // reactive power estimate
  energieZi()     // daily energy (kWh)
}
```
### Class `ContorInteligent`
Handles all devices and performs global calculations:
```js
class ContorInteligent {
  adauga(dispozitiv)
  profilZilnic()          // 24h power profile
  energiiPeDispozitiv()   // kWh/month per device
  totaluri()              // aggregate KPIs
  afiseazaKPI()           // displays current values
  deseneazaLoadCurve()    // draws load curve (line chart)
  deseneazaPie()          // draws energy distribution pie chart
}
```

---

## Visualizations

- **Load Curve (Line Chart)** — total active power vs. hour (0–23)  
- **Energy Pie Chart** — share of monthly energy per device  
- Fully redrawn dynamically using Canvas with `ResizeObserver`

---

## Running the Project

### Option 1 — Local (Recommended)

1. **Download or clone this repository:**

```bash
git clone https://github.com/your-username/smart-energy-meter.git
```

2. **Open** `smart_energy_meter_stacked.html` **in your browser.**

No server required — everything runs client-side.

---

### Option 2 — GitHub Pages

1. Push this project to your GitHub repository.  
2. Go to **Settings → Pages → Source: main branch.**  
3. Your app will be live at:

[https://your-username.github.io/smart-energy-meter](https://your-username.github.io/smart-energy-meter)

---

## Files in This Repository

| File | Description |
|------|--------------|
| `smart_energy_meter_stacked.html` | Complete web app (HTML, CSS, JS integrated) |
| `README.md` | Project documentation |

---

## Example Preset Devices

| Device | Power (W) | PF | Hours/day | Standby (W) |
|--------|------------|----|------------|--------------|
| Fridge | 120 | 0.9 | 24 | 0 |
| TV LED | 80 | 0.95 | 4 | 3 |
| Laptop | 60 | 0.95 | 6 | 2 |
| AC | 900 | 0.9 | 5 | 1 |
| Lights | 6×10 | 1.0 | 6 | 0 |

---

## Formulas Used

Daily energy:

$$
E_{zi} = \frac{1}{1000} \sum_{h=0}^{23} P(h)
$$

Monthly energy:

$$
E_{lună} = E_{zi} \times Zile
$$

Monthly cost:

$$
Cost_{lună} = E_{lună} \times Tarif
$$

CO₂ emissions:

$$
CO_{2(lună)} = E_{lună} \times Fact_{CO_{2}}
$$

---

## Technologies Used

- **HTML5** – Structure and layout  
- **CSS3 (Grid & Flex)** – Styling and responsive design  
- **JavaScript (OOP)** – Core logic and visualization  
- **Canvas API** – Custom chart rendering  
- **ResizeObserver API** – Dynamic canvas scaling

---

## Conclusion

The **Smart Energy Meter** provides a clear, interactive way to understand household energy behavior.  
It combines **data modeling**, **visual analytics**, and **environmental impact estimation** in a single educational tool.

### Possible extensions:
- Trifazic simulation  
- Energy efficiency suggestions  
- Data export (CSV or PDF)  
- Integration with IoT sensors  
