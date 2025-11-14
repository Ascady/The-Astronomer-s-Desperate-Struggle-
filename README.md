# The-Astronomer-s-Desperate-Struggle

Enter the star's information as instructed and calculate the probability of discovering an exoplanet.

# Exoplanet Type Predictor

A heuristic-based tool for predicting the probability distribution of planet types around a given star based on its physical characteristics.

## Overview

This tool estimates the likelihood of different planet types forming around a star using empirically-derived heuristics based on stellar properties. It outputs probability distributions across seven categories: high-metallicity rocky planets, standard rocky planets, low-metallicity rocky planets, low-mass gas giants, high-mass gas giants, Kuiper belt objects (dwarf planets), and the probability of no planets existing.

## Quick Start

```bash
python exoplanet_predictor.py
```

Input the following parameters when prompted:
- **Variability Type**: e.g., "BY Draconis", "Delta Scuti", or leave blank
- **Binary Type**: e.g., "Alpha Centauri-type", "Sirius-type", or leave blank
- **Stellar Mass**: in solar masses (M☉)
- **Metallicity**: [Fe/H] value (solar = 0.0)
- **Age**: in gigayears (Gyr)
- **Spectral Type**: e.g., "G2V", "M5V", "K0III"

### Example Input

```
Variability Type: 
Binary Type: 
Stellar Mass (in solar masses): 1.0
Metallicity [Fe/H]: 0.0
Age (in Gyr): 4.6
Spectral Type (e.g., G2V, M5V, K5III): G2V
```

### Example Output

```
Predicted Planet Probabilities:
H_metal     : 15.3%
Rock        : 15.3%
L_metal     : 15.3%
L_gas       : 23.0%
H_gas       : 23.0%
Kuiper      : 8.0%
No_planet   : 0.0%
```

## Methodology

### Core Approach

The predictor uses a scoring system that combines multiple astrophysical factors:

1. **Metallicity Effects**: Higher metallicity favors all planet types, especially rocky and gas giants
2. **Stellar Mass**: Affects disk mass and planet formation efficiency
3. **Spectral Type**: Converted to numerical scale for systematic effects
4. **Luminosity Class**: Giants, dwarfs, and main-sequence stars have different environments
5. **Age**: Young systems favor Kuiper belt objects; old systems show depletion
6. **Binary Configuration**: Dynamical disruption reduces planet formation probability
7. **Stellar Variability**: Activity and pulsations can inhibit or disrupt planets

### Spectral Type Scale

Stars are converted to a numerical scale for systematic analysis:

- Y0-Y9: 1-10
- T0-T9: 11-20
- L0-L9: 21-30
- M0-M9: 31-40 (TRAPPIST-1: M8V ≈ 32)
- K0-K9: 41-50 (Tau Ceti: K0V ≈ 50)
- G0-G9: 51-60 (Sun: G2V = 58)
- F0-F9: 61-70
- A0-A9: 71-80
- B0-B9: 81-90
- O0-O9: 91-100
- W (Wolf-Rayet): 999

Stars hotter than G-type face exponentially increasing `no_planet` probability due to short main-sequence lifetimes and harsh radiation environments.

### Planet Categories

- **h_metal**: High-metallicity rocky planets (Mercury-like)
- **rock**: Standard rocky planets (Earth/Venus-like)
- **l_metal**: Low-metallicity rocky/icy planets (Mars-like, asteroid belt objects)
- **l_gas**: Low-mass gas giants (Neptune/Uranus-like)
- **h_gas**: High-mass gas giants (Jupiter/Saturn-like)
- **kuiper**: Kuiper belt objects and dwarf planets (Pluto-like)
- **no_planet**: Probability of no planetary system

### Key Heuristics

#### Metallicity
Higher [Fe/H] dramatically increases solid material availability:
- Rocky planets: +30-60 points per +0.1 dex
- Gas giants: +15-60 points per +0.1 dex (steeper scaling)
- Below [Fe/H] < -0.3, `no_planet` probability increases

#### Stellar Mass Regimes
- **< 0.085 M☉**: Brown dwarfs - favor rocky planets, suppress gas giants
- **0.085-0.1 M☉**: Ultra-cool dwarfs (TRAPPIST-1 regime) - strong rocky preference
- **0.1-0.5 M☉**: Low-mass M-dwarfs - rocky and ice favored
- **1.5-2.5 M☉**: Massive stars - favor gas giants
- **> 2.5 M☉**: Very massive stars - exponentially increasing `no_planet`

#### Spectral Type Effects
- **O/B/A stars**: Short lifespans, harsh UV, high `no_planet` probability
- **F stars**: Moderate, slightly unfavorable for rocky planets
- **G/K stars**: Optimal for diverse planetary systems (Solar analogs)
- **M dwarfs**: Favor compact rocky systems, suppress distant ice
- **L/T/Y dwarfs**: Minimal planet formation capability

#### Kuiper Belt Evolution
Peak abundance occurs around 1 Gyr, then gradually depletes:
- Young systems (< 0.1 Gyr): Still forming
- Prime age (0.5-3 Gyr): Peak abundance
- Old systems (> 10 Gyr): Depleted from collisional grinding

Kuiper belts are most abundant around K-type stars (optimal disk conditions).

#### Luminosity Class
- **Class V (Main Sequence)**: Favorable for planets, age-dependent Kuiper evolution
- **Class IV (Subgiants)**: Slight rocky planet penalty
- **Class III (Giants)**: Evolving stars, disrupted systems
- **Class I/II (Supergiants)**: Extreme environments, very high `no_planet`
- **Class VI (Subdwarfs)**: Metal-poor, reduced planet probability
- **Class D (White Dwarfs)**: Post-evolution remnants, high `no_planet`

#### Binary System Penalties
Dynamical stability depends on binary configuration:
- **W UMa (contact)**: 0.2× multiplier, -50 penalty (extreme disruption)
- **Beta Lyrae**: 0.6× multiplier, -20 penalty
- **Castor-type**: 0.3× multiplier, -60 penalty (sextuple system)
- **Sirius-type**: 0.5× multiplier, -20 penalty
- **Alpha Centauri-type**: 0.8× multiplier (wide separation, relatively stable)

#### Stellar Variability Penalties
Activity and pulsations affect planetary habitability:
- **Mild rotation/starspots**: 0.9× (minimal impact)
- **BY Draconis/RS CVn**: 0.8-0.85× (young active stars)
- **Delta Scuti/Gamma Dor**: 0.8× (mild pulsations)
- **T Tauri**: 0.8× (pre-main sequence, still forming)
- **Flare stars**: 0.5× (frequent high-energy events)
- **Cepheids/Mira**: 0.2-0.3× (evolved pulsators, extreme)

## Validation: Test Cases

### Sun (G2V)
**Input**: 1.0 M☉, [Fe/H] = 0.0, Age = 4.6 Gyr, G2V

**Output**: 
- Rocky: 45.9% (Mercury, Venus, Earth, Mars + asteroid belt)
- Gas: 46.0% (Jupiter, Saturn, Uranus, Neptune)
- Kuiper: 8.0% (Pluto, dwarf planets)

**Assessment**: Excellent match - roughly equal terrestrial/giant split

### Kepler-90 (G0V)
**Input**: 1.2 M☉, [Fe/H] ≈ +0.0, Age ≈ 2 Gyr, G0V

**Output**:
- Rocky: 34.0%
- Gas: 45.9%
- Kuiper: 6.9%
- No planet: 13.2%

**Reality**: 8 confirmed planets (4 rocky inner + 4 larger outer)

**Assessment**: Good prediction - favors gas giants slightly, matches observed diversity

### Tau Ceti (G8V)
**Input**: 0.78 M☉, [Fe/H] ≈ -0.5, Age ≈ 5.8 Gyr, G8V

**Output**:
- L_metal: 41.0%
- Kuiper: 31.7%
- Rock: 13.7%
- H_gas: 13.7%

**Reality**: Metal-poor star with 4 candidate rocky planets and massive debris disk (10× Solar System)

**Assessment**: Excellent - high Kuiper score correctly predicts famous debris disk, l_metal dominance reflects low metallicity

### TRAPPIST-1 (M8V)
**Input**: 0.089 M☉, [Fe/H] ≈ 0.0, Age ≈ 7.6 Gyr, M8V

**Output**:
- Rock: ~50%
- H_metal + L_metal: ~50%
- Gas giants: ~0%

**Reality**: 7 rocky Earth-sized planets in compact configuration

**Assessment**: Correct - ultra-cool dwarfs favor rocky planets, suppress gas giants

### Castor (A1V + A2Vm + ... sextuple)
**Input**: A-type stars in complex sextuple system

**Output**:
- No_planet: 100%

**Reality**: No known planets (likely impossible to form/maintain)

**Assessment**: Perfect - correctly identifies hostile multiple star environment

## Input Parameters

### Variability Type
Common values for exoplanet hosts:
- `""` (blank): No known variability
- `"rotation"` / `"starspot"`: Mild spot activity
- `"BY Draconis"`: Young active K/M dwarfs
- `"RS CVn"`: Active chromosphere binaries
- `"Delta Scuti"`: Short-period pulsators
- `"T Tauri"`: Pre-main sequence stars

Avoid extreme types (Cepheids, Mira variables) unless specifically analyzing evolved stars.

### Binary Type
- `""` (blank): Single star
- `"Alpha Centauri-type"`: Wide binary, relatively stable
- `"Sirius-type"`: Intermediate separation
- `"Castor-type"`: Complex multiple system
- `"W UMa"`: Contact binary, extremely disruptive

### Stellar Mass
In solar masses (M☉). Range: 0.08 M☉ (hydrogen-burning limit) to ~100 M☉ (theoretical upper limit).

Common ranges:
- M-dwarfs: 0.08-0.6 M☉
- K-dwarfs: 0.6-0.9 M☉
- G-dwarfs: 0.9-1.1 M☉
- F-dwarfs: 1.1-1.4 M☉
- A-dwarfs: 1.4-2.1 M☉

### Metallicity [Fe/H]
Logarithmic scale relative to solar composition:
- [Fe/H] = 0.0: Solar metallicity
- [Fe/H] = +0.3: Twice solar (metal-rich)
- [Fe/H] = -0.3: Half solar (metal-poor)

Typical range for exoplanet hosts: -1.0 to +0.5

### Age
In gigayears (Gyr). Consider main-sequence lifetime:
- Sun (G2V): ~10 Gyr total MS lifetime
- M-dwarfs: 100+ Gyr (essentially ageless)
- A-stars: <1 Gyr (short-lived)

### Spectral Type
Standard MK classification: `[Class][Subtype][Luminosity]`

Examples:
- `"G2V"`: Sun (G-class, subtype 2, main sequence)
- `"M5V"`: Red dwarf (M-class, subtype 5, main sequence)
- `"K0III"`: Orange giant (K-class, subtype 0, giant)
- `"A0V"`: A-type main sequence

Supported classes: O, B, A, F, G, K, M, L, T, Y, W
Supported luminosity: I, II, III, IV, V, VI, D

## Output Interpretation

Percentages represent **relative probability** of each planet type existing in the system. They are not absolute detection probabilities.

### High Scores (>30%)
Dominant planet type(s) expected in the system

### Moderate Scores (10-30%)
Plausible secondary populations

### Low Scores (<10%)
Unlikely but possible (edge cases)

### Zero Scores
Effectively impossible under given conditions (after safety floor)

### No_Planet Score
High values (>50%) suggest:
- Hostile stellar environment (massive stars, evolved giants)
- Extreme binarity (close/contact systems)
- Very low metallicity
- Extreme variability

## Limitations

### Heuristic Nature
This tool uses **empirical approximations**, not detailed formation simulations. Results are probabilistic estimates, not definitive predictions.

### What This Tool Does NOT Account For
- Orbital dynamics and stability (e.g., mean motion resonances)
- Migration mechanisms (Type I/II migration)
- Late Heavy Bombardment events
- Specific disk properties (mass, density profiles)
- Planetary atmospheres and habitability
- Detection biases in observational data

### Known Edge Cases
- **Very young systems (< 0.1 Gyr)**: Predictions are speculative as planets are still forming
- **Post-AGB stars**: Complex evolutionary phases not well-modeled
- **Hierarchical multiple systems**: Only simple binary penalties applied
- **Extreme metallicities** ([Fe/H] < -1.5 or > +0.5): Extrapolation beyond calibration range

### Calibration Basis
Heuristics are primarily calibrated to:
- Solar System
- Well-characterized exoplanet systems (Kepler, TESS discoveries)
- Statistical trends from exoplanet catalogs

## Future Improvements

### Planned Features
- [ ] Migration modeling (hot Jupiters from cold formation)
- [ ] Habitable zone calculations
- [ ] Planet mass estimates (not just type)
- [ ] Observational detectability scores
- [ ] Integration with stellar evolution models
- [ ] Machine learning calibration from exoplanet databases

### Potential Enhancements
- GUI interface for easier parameter input
- Batch processing for multiple stars
- Visualization of probability distributions
- Export to standard formats (JSON, CSV)
- Integration with astronomical databases (SIMBAD, Exoplanet Archive)

## Technical Details

### Algorithm Flow
1. Parse spectral type into numerical scale
2. Apply base metallicity effects
3. Apply stellar mass regime modifiers
4. Apply spectral type systematic effects
5. Apply luminosity class modifiers
6. Apply age-dependent Kuiper evolution
7. Apply binary system penalties (multiplicative + subtractive)
8. Apply variability penalties (multiplicative + subtractive)
9. Apply global Kuiper belt dampening (0.2×)
10. Apply safety floor (max with 0)
11. Normalize to 100% probability distribution

### Penalty System
Penalties are applied as **combined multiplicative and subtractive** effects:
```python
score = (base_score × multiplier) - fixed_penalty
```

This allows severe cases (W UMa binaries, Cepheids) to drive scores to zero even with high base values.

### Safety Floor
All negative scores are floored to zero before normalization. This prevents nonsensical negative probabilities.

### Normalization
Final scores are normalized so all categories sum to 100%. If all scores are zero (extreme hostile environment), `no_planet` is set to 100%.

## License & Attribution

This tool is provided for educational and research purposes. When using results in publications, please cite appropriately and note the heuristic nature of predictions.

---

**Version**: 2.0  
**Last Updated**: 2025  
**Author**: [Your Name/Organization]  
**Contact**: [Contact Information]
