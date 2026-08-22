# Orbit Compass

A spacecraft trajectory planning application that calculates optimal 
launch windows and orbital transfer paths using real orbital mechanics.

---



![First Screenshot](screenshots/globe.png



![Second Screenshot](screenshots/trajectory.png)



![Third Screenshot](screenshots/results.png)



---

## What it does

Select a launch site, choose a destination, and the app calculates 
the most fuel-efficient trajectory and best upcoming launch windows.

**Launch sites:**
- 🇮🇳 Satish Dhawan Space Centre, Sriharikota — ISRO
- 🇺🇸 Kennedy Space Center, Florida — NASA  
- 🇫🇷 Guiana Space Centre, Kourou — ESA
- 🇷🇺 Baikonur Cosmodrome — Roscosmos

**Destinations:**
- 🛰 Low Earth Orbit (400 km)
- 🌙 Lunar Transfer Orbit
- 🔴 Mars
- ⭐ Venus Flyby

---

## Physics

The calculations use two-body orbital mechanics and the 
patched conic approximation — the standard method for 
preliminary interplanetary mission design.

### Hohmann Transfer

The minimum energy path between two circular orbits.
Transfer ellipse semi-major axis:

```
a = (r₁ + r₂) / 2
```

Burn magnitudes from the vis-viva equation:

```
v = √(GM · (2/r − 1/a))
```

### Patched Conic Approximation

For interplanetary transfers, the trajectory is split into 
three segments:

```
Earth departure hyperbola
        ↓
Heliocentric Hohmann ellipse (~259 days for Mars)
        ↓
Mars arrival hyperbola
```

Each segment is solved as a two-body problem and patched 
at the sphere of influence boundary.

### Launch Window Optimization

For Mars, the required Earth–Mars phase angle at departure:

```
φ = π − ω_Mars × t_transfer
```

This is why Mars windows open only every ~26 months 
(synodic period = 779.9 days).

### Constants (JPL DE430)

| Constant | Value |
|---|---|
| GM_Sun | 1.32712440018 × 10¹¹ km³/s² |
| GM_Earth | 398600.4418 km³/s² |
| GM_Mars | 42828.37 km³/s² |
| 1 AU | 1.495978707 × 10⁸ km |

---

## Mission Output

For each trajectory the app calculates:

| Parameter | Description |
|---|---|
| Total Δv | Total fuel requirement (km/s) |
| Flight time | Transit duration (days) |
| C3 energy | Launch energy = v∞² (km²/s²) |
| Mission phases | Δv breakdown per burn |
| Launch windows | Best upcoming dates ranked by Δv |

### Example — SDSC to Mars (Jan 2027 window)

| Phase | Δv |
|---|---|
| Ascent to LEO parking orbit | 8.876 km/s |
| Trans-Mars Injection (TMI) | 0.538 km/s |
| Heliocentric cruise (259 days) | 0.000 km/s |
| Mars Orbit Insertion (MOI) | 0.919 km/s |
| **Total** | **10.333 km/s** |

---

## Run Locally

```bash
git clone https://github.com/yourusername/orbit-compass
cd orbit-compass
npm install
npm run dev
```

Open **http://localhost:8080**

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI Framework | React 19 |
| Language | TypeScript |
| 3D Visualization | Three.js |
| Build Tool | Vite |
| Styling | Tailwind CSS |
| Physics Engine | Custom — two-body + patched conic |

---

## Accuracy

The physics engine gives ±5% accuracy — sufficient for:
- Comparing mission options
- Identifying optimal launch windows  
- Computing Δv budgets
- Preliminary mission analysis (Phase A level)

The SDSC → Mars numbers closely match published 
Mangalyaan (MOM) trajectory data.

---

## Background

Built to deepen my understanding of astrodynamics after 
completing B.Tech in Aerospace Engineering (Avionics) 
from NIMS Institute of Engineering & Technology, Jaipur.

The physics core is modular — it can be replaced with 
NASA GMAT or ESA poliastro for higher fidelity without 
changing the UI layer.

---

## License

MIT — free to use, modify, and distribute.
