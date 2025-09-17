# Time Domain Features Documentation

## Overview

Time domain features capture statistical and morphological characteristics of EEG signals directly from the amplitude-time representation without frequency transformation. These features are computationally efficient and provide insights into signal dynamics, complexity, and variability.

## Statistical Features

### Mean
**Features:** `time_mean_mean`, `time_mean_std`, `time_mean_max`, `time_mean_min`

- **Calculation**: Average amplitude across time points
- **Interpretation**:
  - Represents DC offset or baseline shift
  - Should be near zero for properly preprocessed EEG
  - Non-zero values may indicate DC drift or referencing issues

### Standard Deviation
**Features:** `time_std_mean`, `time_std_std`, `time_std_max`, `time_std_min`

- **Calculation**: Square root of variance: √[Σ(x - mean)² / N]
- **Interpretation**:
  - Measures signal amplitude variability
  - Higher values indicate greater signal power
  - Proxy for overall brain activity level

### Variance
**Features:** `time_var_mean`, `time_var_std`, `time_var_max`, `time_var_min`

- **Calculation**: Average squared deviation from mean: Σ(x - mean)² / N
- **Interpretation**:
  - Square of standard deviation
  - Represents signal power in time domain
  - More sensitive to outliers than standard deviation

### Skewness
**Features:** `time_skewness_mean`, `time_skewness_std`, `time_skewness_max`, `time_skewness_min`

- **Calculation**: Third standardized moment: E[(X - μ)³] / σ³
- **Interpretation**:
  - Measures asymmetry of amplitude distribution
  - Positive skewness: tail extends toward positive values (upward spikes)
  - Negative skewness: tail extends toward negative values (downward spikes)
  - Zero skewness: symmetric distribution

### Kurtosis
**Features:** `time_kurtosis_mean`, `time_kurtosis_std`, `time_kurtosis_max`, `time_kurtosis_min`

- **Calculation**: Fourth standardized moment: E[(X - μ)⁴] / σ⁴
- **Interpretation**:
  - Measures "peakedness" and tail weight of amplitude distribution
  - High kurtosis: sharp peak with heavy tails (presence of outliers/spikes)
  - Low kurtosis: flat distribution
  - Normal distribution has kurtosis = 3 (excess kurtosis = 0)

## Signal Characteristics

### Root Mean Square (RMS)
**Features:** `time_rms_mean`, `time_rms_std`, `time_rms_max`, `time_rms_min`

- **Calculation**: √[Σ(x²) / N]
- **Interpretation**:
  - Measures average signal power
  - Robust measure of signal amplitude
  - Less affected by DC offset than variance

### Peak-to-Peak Amplitude
**Features:** `time_peak_to_peak_mean`, `time_peak_to_peak_std`, `time_peak_to_peak_max`, `time_peak_to_peak_min`

- **Calculation**: Maximum value - Minimum value
- **Interpretation**:
  - Total amplitude range of signal
  - Sensitive to artifacts and extreme values
  - Useful for detecting high-amplitude events

### Zero Crossing Rate
**Features:** `time_zero_crossings_mean`, `time_zero_crossings_std`, `time_zero_crossings_max`, `time_zero_crossings_min`

- **Calculation**: Number of sign changes per second: count(sign(xᵢ) ≠ sign(xᵢ₊₁)) / duration
- **Interpretation**:
  - Crude estimate of dominant frequency
  - Higher rate indicates higher frequency content
  - Low rate suggests slow waves or DC shifts

## Hjorth Parameters

### Hjorth Activity
**Features:** `time_hjorth_activity_mean`, `time_hjorth_activity_std`, `time_hjorth_activity_max`, `time_hjorth_activity_min`

- **Calculation**: Variance of the signal (scaled by 10⁸)
- **Interpretation**:
  - Represents total signal power
  - First Hjorth parameter
  - Equivalent to variance but traditionally called "activity" in Hjorth framework

### Hjorth Mobility
**Features:** `time_hjorth_mobility_mean`, `time_hjorth_mobility_std`, `time_hjorth_mobility_max`, `time_hjorth_mobility_min`

- **Calculation**: std(first derivative) / std(signal)
- **Interpretation**:
  - Represents mean frequency of the signal
  - Estimates the proportion of standard deviation due to frequency
  - Higher values indicate higher dominant frequencies
  - Dimensionless ratio

### Hjorth Complexity
**Features:** `time_hjorth_complexity_mean`, `time_hjorth_complexity_std`, `time_hjorth_complexity_max`, `time_hjorth_complexity_min`

- **Calculation**: mobility(first derivative) / mobility(signal)
- **Interpretation**:
  - Represents frequency spread or bandwidth
  - Indicates how similar the signal is to a pure sine wave
  - Complexity = 1: pure sine wave
  - Complexity > 1: multiple frequency components
  - Measures signal irregularity

## Complexity Measures

### Line Length
**Features:** `time_line_length_mean`, `time_line_length_std`, `time_line_length_max`, `time_line_length_min`

- **Calculation**: Sum of absolute consecutive differences normalized by sample count: Σ|xᵢ₊₁ - xᵢ| / N
- **Interpretation**:
  - Measures signal complexity and variability
  - Sensitive to both amplitude and frequency changes
  - Higher values indicate more oscillatory or irregular activity
  - Useful for seizure detection (increases during ictal periods)

### Non-Linear Energy
**Features:** `time_nonlinear_energy_mean`, `time_nonlinear_energy_std`, `time_nonlinear_energy_max`, `time_nonlinear_energy_min`

- **Calculation**: Mean of [x(t)² - x(t-1) × x(t+1)]
- **Interpretation**:
  - Measures signal predictability
  - Sensitive to frequency changes and spikes
  - Higher values indicate rapid signal changes
  - Originally developed for speech processing, adapted for EEG
  - Emphasizes high-frequency components while suppressing low frequencies

## Channel Aggregation

Each feature is calculated independently for every channel, then aggregated:

- **Mean across channels** (`_mean`): Global brain activity measure
- **Std across channels** (`_std`): Spatial heterogeneity
- **Max across channels** (`_max`): Peak regional activity
- **Min across channels** (`_min`): Minimum regional activity

## Clinical Applications

### Seizure Detection
- **Increased line length**: Characteristic of seizure onset
- **High non-linear energy**: Indicates epileptic spikes
- **Altered Hjorth parameters**: Changes in frequency content
- **Increased variance/RMS**: Higher amplitude during ictal period

### Sleep Stage Classification
- **Hjorth mobility**: Differentiates REM from non-REM
- **Zero crossing rate**: Lower in deep sleep
- **Variance patterns**: Stage-specific amplitude characteristics

### Artifact Detection
- **High peak-to-peak**: Eye blinks, movement artifacts
- **Extreme skewness/kurtosis**: Non-physiological distributions
- **Abnormal mean**: DC drift or electrode problems

## Feature Interpretation Guidelines

### Normal EEG Patterns
- **Near-zero mean**: Properly referenced signal
- **Moderate variance**: Normal amplitude range (10-100 μV)
- **Skewness near zero**: Symmetric amplitude distribution
- **Hjorth complexity 1-2**: Mix of frequencies typical for EEG

### Pathological Patterns
- **Increased line length**: Seizures, high-frequency activity
- **High kurtosis**: Epileptic spikes, sharp waves
- **Low complexity**: Monotonous rhythms, coma patterns
- **Asymmetric skewness**: Focal abnormalities

### State Changes
- **Variance reduction**: Sleep deepening, anesthesia
- **Mobility changes**: Frequency shifts with alertness
- **Complexity variations**: Cognitive load, task engagement

## Advantages

### Computational Efficiency
- No frequency transformation required
- Real-time calculation feasible
- Low memory requirements
- Linear time complexity O(N)

### Interpretability
- Direct physical meaning
- Intuitive clinical interpretation
- Well-established in literature
- Robust to short segments

### Complementary Information
- Captures temporal dynamics missed by spectral analysis
- Sensitive to transient events
- Preserves phase information
- No windowing artifacts

## Technical Considerations

### Preprocessing Requirements
- Remove DC offset for accurate statistics
- High-pass filter for stable Hjorth parameters
- Adequate sampling rate for derivative calculations
- Sufficient segment length for reliable statistics

### Parameter Sensitivity
- **Segment length**: Affects statistical reliability
- **Sampling rate**: Influences derivative-based features
- **Amplitude scaling**: Important for absolute features
- **Reference montage**: Affects mean and variance

### Quality Indicators
- Consistent statistics across channels indicates good signal quality
- Extreme values suggest artifacts
- Zero or near-zero variance indicates flat signal/bad electrode
- Very high complexity may indicate noise contamination

## Regional Analysis

Time domain features can be calculated for specific brain regions:
- Captures regional amplitude and complexity differences
- Useful for lateralization (left vs right comparisons)
- Identifies focal vs generalized patterns
- Features follow pattern: `{prefix}_{region}_time_{feature}_{aggregation}`

## Feature Correlations

### Related Features
- Variance ≈ Activity ≈ RMS² (for zero-mean signals)
- Line length correlates with mobility
- Peak-to-peak sensitive to outliers affecting kurtosis
- Zero crossings relates to mobility (frequency content)

### Independent Information
- Skewness independent of variance
- Complexity independent of amplitude
- Non-linear energy captures unique dynamics
- Hjorth parameters form orthogonal description