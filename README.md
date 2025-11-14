# Planet Type Predictor

A Python tool that predicts the probability distribution of different planet types around a star based on its physical characteristics.

## Usage

### Input Parameters

- **Variability Type**: Star's variability classification (e.g., "BY Draconis", "Delta Scuti", "Flare", or leave blank for none)
- **Binary Type**: Binary system classification (e.g., "Alpha Centauri", "Sirius", or leave blank for single star)
- **Stellar Mass**: Mass in solar masses (e.g., 1.0 for Sun-like)
- **Metallicity [Fe/H]**: Metal content relative to the Sun (e.g., 0.0 for solar, +0.2 for metal-rich, -0.5 for metal-poor)
- **Age**: Star's age in gigayears (Gyr) (e.g., 4.6 for Sun-like)
- **Spectral Type**: Standard spectral classification (e.g., "G2V", "M5V", "K5III")

## Output Categories

The tool returns probability percentages for each planet type:

- **H_metal**: High-metal content rocky planets (super-Earths, iron-rich)
- **Rock**: Standard rocky terrestrial planets (Earth-like, Mars-like)
- **L_metal**: Low-metal worlds (carbon planets, water worlds, ice planets)
- **L_gas**: Light gas giants (Jupiter-type, hydrogen/helium dominated)
- **H_gas**: Heavy gas giants (Neptune-type, ammonia/methane rich)
- **Kuiper**: Kuiper belt objects (Pluto-like dwarf planets in outer regions)
- **No_planet**: Probability of no planetary system

## Example

```
Planet Type Predictor
Variability Type: 
Binary Type (e.g., Alpha Centauri-type, Procyon-type): 
Stellar Mass (in solar masses): 1.0
Metallicity [Fe/H]: 0.0
Age (in Gyr): 4.6
Spectral Type (e.g., G2V, M5V, K5III): G2V

Predicted Planet Probabilities:
H_metal     : 15.2%
Rock        : 18.7%
L_metal     : 12.3%
L_gas       : 28.4%
H_gas       : 21.1%
Kuiper      : 3.8%
No_planet   : 0.5%
```

## Key Factors

The model considers:
- **Stellar mass**: Affects planet formation efficiency and types
- **Metallicity**: Higher metallicity favors planet formation
- **Age**: Young systems may retain more Kuiper objects
- **Spectral type**: Temperature and luminosity class affect habitability zones
- **Variability**: Stellar activity can disrupt planetary systems
- **Binary companions**: Gravitational interactions affect planet stability

## Notes

- All probabilities sum to 100%
- Debug output shows raw scores before normalization
- Designed for main-sequence and evolved stars (not exotic objects)
**Version**: 2.0  
**Last Updated**: 2025  
**Author**: Ikara
