# Private Federated Learning for Smart Meters

Testing whether **secure aggregation + distributed differential privacy** can protect household electricity data in federated learning, against attacks built for time-series data.

## Problem
In federated learning, smart meters train a shared forecasting model by sending model updates to a server instead of raw readings. But gradient inversion attacks can reconstruct those readings from the updates. Stolen load curves reveal when people are home and which appliances they use.

- **Differential privacy (DP):** each meter adds noise to its update. This protects privacy but hurts accuracy.
- **Secure aggregation (SecAgg):** meters hide their updates with paired random masks that cancel out when summed, so the server only sees the total. This costs extra computation and communication.

Combining them lets each meter add only a small share of the noise, while the server only ever sees the noised sum.

## Threat Model
- **Protected:** each household's raw readings.
- **Attacker:** an honest-but-curious server that runs the protocol correctly but tries to reconstruct household data from what it receives. It knows the model and the defenses.
- **Out of scope (for now):** malicious servers, fake clients, side channels.

## Protection Levels Compared
1. **No defense** (baseline)
2. **Local DP only:** each meter adds full noise to its own update
3. **SecAgg+ with distributed DP:** each meter adds a small integer noise share, then masks its update

Each level is measured on **privacy** (attack success), **forecast accuracy**, and **cost**.

## What's New
- Attacks designed for time-series data (TS-Inverse, learned inverters) tested against aggregated updates from realistic small neighborhoods (5–20 houses)
- A client-side distributed-DP step added to Flower's SecAgg+ (not built into Flower)
- Later: cost measurements on a real meter-class microcontroller

## This Semester's Plan
- [ ] Train a small load-forecasting model on the London Smartmeter dataset
- [ ] Simulate households in [Flower](https://flower.ai)
- [ ] Implement all three protection levels
- [ ] Attack each level with TS-Inverse and/or a learned inverter, with 5, 10 and 20 houses
- [ ] (Stretch) Time key agreement on a microcontroller

## Future Work
Meter dropouts during rounds, and full microcontroller measurements (instructions, RAM, energy including the radio).

## Key References
- Meijer et al., *TS-Inverse* (2025)
- Wu et al., learned gradient inverters (UAI 2023)
- Bonawitz et al., *Practical Secure Aggregation* (2017); Bell et al., *SecAgg+* (2020)
- Delgado Fernandez et al., FL for household load forecasting (2021)

*Status: early stage, part of an Applied Cryptography course project.*
