# Wavelet Features Documentation

## Overview

Wavelet transform features provide multi-resolution time-frequency analysis of signals, capturing both temporal and spectral characteristics at different scales. Unlike Fourier transforms, wavelets maintain temporal localization while analyzing frequency content, making them ideal for non-stationary signals like EEG.

## Wavelet Decomposition Parameters

### Default Configuration
- **Wavelet Type**: Daubechies 4 ('db4')
  - Compact support with 4 vanishing moments
  - Good balance between temporal and frequency resolution
  - Commonly used for biomedical signal analysis

- **Decomposition Levels**: 5 levels
  - Each level represents different frequency bands
  - Higher levels = lower frequencies
  - Number of levels determines frequency resolution

## Discrete Wavelet Transform (DWT) Process

### Decomposition Structure
The signal is decomposed into approximation and detail coefficients:
- **Level 0 (A5)**: Approximation coefficients (lowest frequencies)
- **Level 1 (D5)**: Detail coefficients (highest decomposition band)
- **Level 2 (D4)**: Detail coefficients
- **Level 3 (D3)**: Detail coefficients  
- **Level 4 (D2)**: Detail coefficients
- **Level 5 (D1)**: Detail coefficients (lowest decomposition band)

### Frequency Band Correspondence
For typical EEG sampling rates (e.g., 256 Hz), approximate frequency ranges:
- **Level 0 (A5)**: 0-4 Hz (Delta range)
- **Level 1 (D5)**: 4-8 Hz (Theta range)
- **Level 2 (D4)**: 8-16 Hz (Alpha range)
- **Level 3 (D3)**: 16-32 Hz (Beta range)
- **Level 4 (D2)**: 32-64 Hz (Low Gamma range)
- **Level 5 (D1)**: 64-128 Hz (High Gamma range)

*Note: Exact frequency ranges depend on sampling rate and wavelet properties*

## Features Calculated per Level

For each decomposition level (0 through 5), five features are extracted:

### 1. Energy
**Features:** `wt_level{N}_energy_mean`, `wt_level{N}_energy_std`

- **Calculation**: Sum of squared coefficients: Σ(coeff²)
- **Interpretation**: 
  - Represents signal power at that scale/frequency band
  - Higher energy indicates stronger activity in that frequency range
  - Energy distribution across levels shows dominant frequency components

### 2. Entropy
**Features:** `wt_level{N}_entropy_mean`, `wt_level{N}_entropy_std`

- **Calculation**: Shannon entropy of absolute coefficient values: -Σ(p × log(p))
  - Where p is the normalized distribution of |coefficients|
- **Interpretation**:
  - Measures randomness/complexity at each scale
  - High entropy: coefficients are uniformly distributed (complex/random)
  - Low entropy: few dominant coefficients (structured/periodic)

### 3. Mean Absolute Coefficients
**Features:** `wt_level{N}_mean_mean`, `wt_level{N}_mean_std`

- **Calculation**: Average of absolute coefficient values: mean(|coefficients|)
- **Interpretation**:
  - Average magnitude of activity at that scale
  - Robust measure of signal strength
  - Less sensitive to outliers than energy

### 4. Standard Deviation
**Features:** `wt_level{N}_std_mean`, `wt_level{N}_std_std`

- **Calculation**: Standard deviation of coefficients
- **Interpretation**:
  - Variability of coefficients at each scale
  - Higher values indicate more variable/dynamic activity
  - Captures amplitude fluctuations within each frequency band

### 5. Maximum Absolute Coefficient
**Features:** `wt_level{N}_max_mean`, `wt_level{N}_max_std`

- **Calculation**: Maximum absolute value of coefficients: max(|coefficients|)
- **Interpretation**:
  - Peak activity at each scale
  - Sensitive to transient high-amplitude events
  - Useful for detecting spikes or sharp waves

## Channel Aggregation

Each feature is calculated for every channel, then aggregated:
- **Mean across channels** (`_mean`): Average feature value across all electrodes
- **Std across channels** (`_std`): Spatial variability of the feature

This dual aggregation captures both:
- Global brain activity patterns (mean)
- Spatial heterogeneity (std)

## Wavelet Packet Entropy

**Feature:** `wt_packet_entropy`

### Calculation Process
1. **Wavelet Packet Decomposition**: 
   - Full binary tree decomposition to level 3
   - Creates 2³ = 8 terminal nodes
   - Each node represents a specific time-frequency atom

2. **Energy Distribution**:
   - Calculate energy for each terminal node: Σ(node_data²)
   - Normalize energies to create probability distribution

3. **Entropy Calculation**:
   - Shannon entropy of normalized energy distribution
   - Formula: -Σ(p × log(p)) where p = node_energy/total_energy

### Interpretation
- **High Packet Entropy**: Energy spread across many time-frequency atoms
  - Indicates complex, broadband activity
  - Common in awake, active states
  
- **Low Packet Entropy**: Energy concentrated in few atoms
  - Suggests dominant rhythmic activity
  - May indicate pathological synchronization

## Clinical Applications

### Seizure Detection
- **Increased low-level energy**: Delta/theta enhancement during seizures
- **Decreased entropy**: More organized/synchronized activity
- **High maximum coefficients**: Presence of epileptic spikes

### Sleep Stage Analysis
- **Level-specific patterns**: Different sleep stages show characteristic wavelet signatures
- **Entropy changes**: REM vs non-REM discrimination
- **Energy redistribution**: Transitions between sleep stages

### Artifact Detection
- **High-frequency energy** (levels 4-5): Muscle artifacts
- **Low-frequency energy** (levels 0-1): Movement artifacts
- **Asymmetric spatial distribution**: Localized artifacts

## Advantages Over Traditional Methods

### Temporal Localization
- Unlike FFT, wavelets preserve when frequency components occur
- Captures transient events (spikes, sharp waves)
- Ideal for non-stationary EEG analysis

### Multi-Resolution Analysis
- Automatic frequency band decomposition
- No need to predefine frequency bands
- Natural separation of rhythms at different scales

### Computational Efficiency
- Fast algorithms (O(N) complexity)
- Decimation reduces data at each level
- Suitable for real-time applications

## Feature Interpretation Guidelines

### Energy Distribution Pattern
- **Normal EEG**: Decreasing energy with increasing level (1/f pattern)
- **Epileptic EEG**: Energy peaks at specific levels
- **Artifacts**: Abnormal energy at extreme levels

### Entropy Patterns
- **Consistent entropy**: Stable cognitive state
- **Decreasing entropy**: Increasing synchronization
- **Variable entropy**: State transitions or instability

### Spatial Consistency
- **Low std across channels**: Global phenomenon
- **High std across channels**: Focal or lateralized activity
- **Level-dependent std**: Frequency-specific spatial patterns

## Technical Notes

### Parameter Selection
- **Wavelet choice** affects time-frequency resolution trade-off
- **Number of levels** limited by signal length (2^levels ≤ signal_length)
- **db4** provides good EEG feature extraction

### Computational Considerations
- Wavelet packet decomposition performed on single channel (computational cost)
- Full DWT performed on all channels
- Features rounded to 12 decimal places for precision

### Quality Indicators
- Zero values indicate processing failure for that channel
- Entropy should be positive (negative values indicate errors)
- Energy values should scale with signal amplitude

## Regional Analysis

Wavelet features can be calculated for specific brain regions:
- Regional decomposition captures local time-frequency patterns
- Useful for identifying region-specific rhythms
- Features follow pattern: `{prefix}_{region}_wt_level{N}_{feature}_{aggregation}`