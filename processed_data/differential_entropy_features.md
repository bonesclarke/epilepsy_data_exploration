# Differential Entropy Features Documentation

## Overview

Differential Entropy (DE) quantifies the uncertainty or randomness of continuous signals in specific frequency bands. Unlike discrete entropy, differential entropy measures the information content of continuous probability distributions. These features are calculated for each frequency band after bandpass filtering.

## Frequency Bands

The differential entropy is calculated for the following frequency bands:
- **Infraslow**: 0.1-0.5 Hz
- **Delta**: 0.5-3.5 Hz
- **Theta**: 3.5-8 Hz
- **Alpha**: 8-13 Hz
- **Low Beta**: 13-20 Hz
- **High Beta**: 20-30 Hz
- **Gamma**: 30-50 Hz
- **High Gamma**: 50-80 Hz
- **Ripples**: 80-100 Hz

## Calculation Method

### Core Differential Entropy Formula

For each frequency band:
1. **Bandpass Filtering**: Signal is filtered to isolate the specific frequency band using FIR filter with firwin design
2. **Variance Calculation**: Variance is computed for each channel's filtered signal
3. **Differential Entropy**: Calculated using the formula for Gaussian-distributed signals:
   - DE = 0.5 × log(2π × e × variance)
   - Where e is Euler's number (≈2.718)
   - Assumes the filtered signal follows a Gaussian distribution

## Statistical Features

### Basic Statistics per Band
**Features:** `de_{band}_mean`, `de_{band}_std`, `de_{band}_median`, `de_{band}_max`, `de_{band}_min`

For each frequency band, the following statistics are computed across channels:

- **Mean** (`de_{band}_mean`): Average differential entropy across all channels
  - Indicates the overall information content in that frequency band
  
- **Standard Deviation** (`de_{band}_std`): Variability of differential entropy across channels
  - Measures spatial heterogeneity of information content
  
- **Median** (`de_{band}_median`): Middle value of differential entropy distribution
  - Robust measure of central tendency, less affected by outliers
  
- **Maximum** (`de_{band}_max`): Highest differential entropy value among channels
  - Identifies peak information content location
  
- **Minimum** (`de_{band}_min`): Lowest differential entropy value among channels
  - Identifies minimal information content location

## Asymmetry Features

### Hemispheric Asymmetry
**Features:** `de_{band}_asymmetry_mean`, `de_{band}_asymmetry_std`

Calculated for configurations with bilateral channel arrangements:

- **Asymmetry Calculation**: 
  - Left hemisphere DE values - Right hemisphere DE values
  - Computed for corresponding channel pairs
  
- **Asymmetry Mean** (`de_{band}_asymmetry_mean`):
  - Average difference between left and right hemisphere differential entropy
  - Positive values indicate higher information content in left hemisphere
  - Negative values indicate higher information content in right hemisphere
  - Important for lateralization analysis
  
- **Asymmetry Standard Deviation** (`de_{band}_asymmetry_std`):
  - Variability of the hemispheric differences
  - Higher values indicate more heterogeneous asymmetry patterns
  - Lower values indicate consistent asymmetry across channel pairs

## Feature Interpretation

### Information Content
- **Higher DE values** indicate greater signal complexity and unpredictability in that frequency band
- **Lower DE values** suggest more regular, predictable oscillations

### Clinical Relevance
- **Frontal Alpha Asymmetry**: Associated with emotional processing and mood states
- **Temporal Lobe DE**: Relevant for seizure detection and localization
- **Global DE Changes**: May indicate altered consciousness states or pathological conditions

### Comparison with Power Features
- While power measures signal amplitude, DE captures signal complexity
- Two signals can have identical power but different DE values based on their predictability
- DE is more sensitive to changes in signal regularity than traditional power analysis

## Regional Analysis

Similar to PSD features, differential entropy features can be calculated for specific brain regions:
- Features are computed for the same regional channel groups (frontal, temporal, parietal, occipital, central, left/right hemispheres)
- Regional features follow the naming pattern: `{prefix}_{region}_de_{band}_{statistic}`
- Example: `seizure_frontal_de_alpha_mean` represents mean alpha band differential entropy in frontal region

## Theoretical Background

### Gaussian Assumption
The formula assumes filtered EEG signals follow a Gaussian distribution, which is reasonable for:
- Narrow-band filtered signals due to central limit theorem
- Background EEG activity without strong transients

### Information Theory Basis
- Differential entropy extends Shannon entropy to continuous distributions
- Measured in nats when using natural logarithm
- Represents the average information needed to specify the signal value within the frequency band

### Relationship to Signal Properties
- DE increases with signal variance (more spread = more uncertainty)
- Independent of signal mean (shift-invariant)
- Sensitive to signal distribution shape through the Gaussian assumption