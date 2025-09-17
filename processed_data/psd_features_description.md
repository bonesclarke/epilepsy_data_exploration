# PSD Features Documentation

## Band Power Features

### Basic Band Power Statistics
**Features:** `psd_{band}_mean`, `psd_{band}_std`, `psd_{band}_cv`

- Calculates power spectral density using multitaper method for the following frequency bands:
  - **Infraslow**: 0.1-0.5 Hz
  - **Delta**: 0.5-3.5 Hz
  - **Theta**: 3.5-8 Hz
  - **Alpha**: 8-13 Hz
  - **Low Beta**: 13-20 Hz
  - **High Beta**: 20-30 Hz
  - **Gamma**: 30-50 Hz
  - **High Gamma**: 50-80 Hz
  - **Ripples**: 80-100 Hz

- **Mean**: Average power in the frequency band across time
- **Std**: Standard deviation of power across channels
- **CV**: Coefficient of variation (std/mean) - measures relative variability

### Relative Band Powers
**Features:** `psd_{band}_relative`

- Proportion of each band's power relative to total power
- Formula: `band_power / total_power`
- Indicates the relative contribution of each frequency band

## Spectral Slope Features

### Broadband Slope
**Features:** `psd_slope_broadband`, `psd_slope_intercept`

- Linear regression slope of log(power) vs log(frequency) in 2-45 Hz range
- Represents the 1/f spectral decay rate
- Intercept provides the offset of the fitted line

### Frequency-Specific Slopes
**Features:** `psd_slope_low`, `psd_slope_high`

- **Low Frequency Slope**: Calculated in 1-8 Hz range
- **High Frequency Slope**: Calculated in 30-45 Hz range
- Captures different decay characteristics across frequency ranges

### Knee Frequency
**Feature:** `psd_knee_freq`

- Frequency where the power spectrum transitions from a steeper to shallower slope
- Fitted using broken power law model: `c - log10(1 + (f/knee)^b1) * b2`
- Indicates the transition point in spectral characteristics

## Complexity Measures

### Spectral Entropy
**Feature:** `psd_spectral_entropy`

- Shannon entropy of normalized PSD
- Formula: `-Σ(p * log(p))` where p is normalized power
- Measures overall spectral complexity/randomness

### Band-wise Entropy
**Features:** `psd_{band}_entropy`

- Entropy calculated within each individual frequency band
- Measures complexity within specific frequency ranges

### Spectral Flatness
**Feature:** `psd_spectral_flatness`

- Ratio of geometric mean to arithmetic mean of PSD
- Formula: `geometric_mean / arithmetic_mean`
- Measures how "white noise-like" the spectrum is (1 = white noise, 0 = pure tone)

## Alpha Peak Features

### Alpha Peak Frequency
**Feature:** `psd_alpha_peak_freq`

- Frequency of maximum power within the 8-13 Hz alpha band
- Indicates the dominant alpha rhythm frequency

### Alpha Peak Power
**Feature:** `psd_alpha_peak_power`

- Power value at the identified alpha peak frequency
- Measures the strength of the alpha rhythm

### Alpha Peak Prominence
**Feature:** `psd_alpha_peak_prominence`

- Ratio of peak power to surrounding power in alpha band
- Formula: `peak_power / surrounding_average_power`
- Indicates how much the alpha peak stands out from background

## Periodic Components

### Aperiodic-Adjusted Band Powers
**Features:** `psd_{band}_periodic`

- Band power after removing 1/f aperiodic component
- Calculation: Subtract fitted 1/f line from original spectrum
- Isolates true oscillatory activity from background spectrum

## Frequency Ratios

### Inter-band Power Ratios
**Features:** Various ratio features

- **Theta/Alpha Ratio** (`psd_theta_alpha_ratio`): Relative dominance of theta vs alpha rhythms
- **Delta/Alpha Ratio** (`psd_delta_alpha_ratio`): Slow wave vs alpha activity balance
- **Beta Ratio** (`psd_beta_ratio`): High beta / low beta power
- **Delta/Theta Ratio** (`psd_delta_theta_ratio`): Ultra-slow vs slow wave balance
- **Alpha/Beta Ratio** (`psd_alpha_beta_ratio`): Rest vs activation balance
- **Low/High Ratio** (`psd_low_high_ratio`): Power below 8 Hz / power above 8 Hz

## Spectral Edge Frequencies

### Percentile Frequencies
**Features:** `psd_sef50`, `psd_sef75`, `psd_sef90`, `psd_sef95`

- Frequency below which X% of total power is contained
- SEF50: Median frequency (50th percentile)
- SEF75: 75th percentile frequency
- SEF90: 90th percentile frequency
- SEF95: 95th percentile frequency

### SEF Spread
**Feature:** `psd_sef_spread`

- Difference between SEF90 and SEF50
- Formula: `SEF90 - SEF50`
- Indicates spectral concentration (narrow vs broad spectrum)

## Spectral Moments

### Spectral Centroid
**Feature:** `psd_spectral_centroid`

- Center of mass of the power spectrum
- Formula: `Σ(frequency * power) / Σ(power)`
- Represents the "average" frequency weighted by power

### Spectral Spread
**Feature:** `psd_spectral_spread`

- Standard deviation around spectral centroid
- Formula: `√[Σ((f - centroid)² * power) / Σ(power)]`
- Measures the spread of power around the centroid

### Spectral Skewness
**Feature:** `psd_spectral_skewness`

- Third standardized moment of the spectrum
- Formula: `Σ((f - centroid)³ * power) / (spread³ * Σ(power))`
- Measures asymmetry of spectral distribution (positive = right-skewed)

### Spectral Kurtosis
**Feature:** `psd_spectral_kurtosis`

- Fourth standardized moment of the spectrum
- Formula: `Σ((f - centroid)⁴ * power) / (spread⁴ * Σ(power))`
- Measures peakedness/tailedness of spectral distribution

## Channel Variability

### Slope Variability Measures
**Features:** `psd_slope_variability`, `psd_slope_min`, `psd_slope_max`

- **Variability**: Standard deviation of spectral slopes across channels
- **Min**: Minimum slope value across channels
- **Max**: Maximum slope value across channels
- Measures spatial consistency of spectral characteristics across recording sites