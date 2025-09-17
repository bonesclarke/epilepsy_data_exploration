# Propagation Features Documentation

## Overview

Propagation features quantify how electrical activity spreads across different brain regions during events like seizures. Two complementary methods are used to capture different aspects of signal propagation: simple threshold-based detection and peak-based detection with derivative analysis.

## Method 1: Simple Propagation Features

### Signal Processing Pipeline

1. **Envelope Extraction**
   - Applies Hilbert transform to obtain analytic signal
   - Calculates instantaneous amplitude (envelope) as magnitude of analytic signal
   - Formula: envelope = |Hilbert(signal)|

2. **Envelope Smoothing**
   - Applies Savitzky-Golay filter for noise reduction
   - Window size: 0.1 seconds (converted to samples)
   - Preserves signal shape while removing high-frequency noise

3. **Onset Detection**
   - Establishes baseline using median of first 0.5 seconds of smoothed envelope
   - Calculates threshold: baseline + 2 × standard deviation of baseline period
   - Onset time: First sample where envelope exceeds threshold

### Features Calculated

#### Propagation Speed Metrics
**Features:** `mean_propagation_speed`, `median_propagation_speed`, `std_propagation_speed`, `max_propagation_speed`, `min_propagation_speed`

- **Speed Calculation**: 
  - Formula: speed = distance / time_delay
  - Distance between electrodes assumed as 10mm
  - Time delays calculated from consecutive onset times (sorted)
  - Only positive delays are used for speed calculation

- **Mean Speed**: Average propagation velocity across all channel pairs
- **Median Speed**: Robust central tendency of propagation velocity
- **Std Speed**: Variability in propagation speeds
- **Max Speed**: Fastest observed propagation
- **Min Speed**: Slowest observed propagation

#### Propagation Event Count
**Feature:** `num_propagation_events`
- Number of valid propagation measurements (positive delays between channels)

#### Onset Timing Features
**Features:** `earliest_onset`, `latest_onset`, `onset_spread`, `mean_onset_delay`, `max_onset_delay`

- **Earliest Onset**: Time of first channel activation
- **Latest Onset**: Time of last channel activation  
- **Onset Spread**: Total duration of propagation (latest - earliest onset)
- **Mean Onset Delay**: Average time between consecutive channel activations
- **Max Onset Delay**: Largest time gap between consecutive activations

## Method 2: Peak-Based Propagation Features

### Enhanced Signal Processing

1. **Envelope Calculation**
   - Same Hilbert transform approach as simple method
   - Smoothing with shorter window (0.05 seconds) for better temporal resolution

2. **Peak Detection**
   - Identifies maximum amplitude point in smoothed envelope for each channel
   - Peak time: time of maximum envelope value

3. **Derivative-Based Onset Detection**
   - Calculates first derivative of smoothed envelope
   - Threshold: 2 × standard deviation of derivative in baseline period (first 0.5s)
   - Onset: First point where derivative exceeds threshold (significant rise)

### Features Calculated

#### Speed Metrics (Onset-Based)
**Features:** `mean_propagation_speed`, `median_propagation_speed`, `std_propagation_speed`, `max_propagation_speed`, `min_propagation_speed`

- Calculated identically to simple method but using derivative-based onsets
- More sensitive to rapid signal changes
- Better captures sharp onset dynamics

#### Onset Temporal Features
**Features:** `onset_spread`, `mean_onset_time`

- **Onset Spread**: Range of onset times across channels
- **Mean Onset Time**: Average onset time across all channels

#### Peak Temporal Features
**Features:** `peak_spread`, `mean_peak_time`

- **Peak Spread**: Range of peak times across channels
  - Indicates synchronization of maximum activity
  - Smaller values suggest more synchronized peaks
  
- **Mean Peak Time**: Average time of peak activity across channels
  - Represents the central time of maximum network activation

## Interpretation Guidelines

### Propagation Speed
- **Normal Range**: Typically 1-100 mm/s for cortical spreading
- **Fast Propagation** (>50 mm/s): May indicate synaptic/axonal transmission
- **Slow Propagation** (<10 mm/s): Suggests local spreading mechanisms
- **High Variability**: Indicates heterogeneous propagation pathways

### Temporal Spread
- **Small Onset Spread** (<0.5s): Rapid, synchronized activation
- **Large Onset Spread** (>2s): Slow, sequential recruitment
- **Peak Spread vs Onset Spread**: 
  - Similar values: Consistent propagation pattern
  - Peak spread < Onset spread: Synchronization after initial propagation

### Method Comparison
- **Simple Method**: 
  - More robust to noise
  - Better for clear, sustained events
  - May miss subtle onsets
  
- **Peak-Based Method**:
  - More sensitive to rapid changes
  - Captures both initiation and maximum activity
  - Derivative approach better for sharp onsets

## Clinical Applications

### Seizure Analysis
- **Focal Onset**: Large onset spread, clear propagation direction
- **Generalized Onset**: Small onset spread, simultaneous activation
- **Secondary Generalization**: Initial delays followed by synchronization

### Network Dynamics
- **Propagation Speed**: Indicates underlying mechanism (synaptic vs volume conduction)
- **Onset-Peak Relationship**: Reveals build-up dynamics of pathological activity
- **Spatial Patterns**: Can identify propagation pathways and epileptogenic zones

## Regional Analysis

Both propagation methods can be applied to regional channel groups:
- Intra-regional propagation (within frontal, temporal, etc.)
- Inter-regional propagation (frontal to temporal, etc.)
- Hemispheric propagation (left to right, or vice versa)
- Regional features follow pattern: `{prefix}_{region}_propagation_{metric}`

## Technical Considerations

### Assumptions
- Fixed inter-electrode distance (10mm) - actual distances may vary
- Linear propagation between adjacent channels
- Signal amplitude reflects local activation

### Limitations
- Volume conduction can create apparent instantaneous "propagation"
- Reference electrode choice affects absolute timing
- Requires sufficient signal-to-noise ratio for reliable detection

### Quality Indicators
- Number of detected onsets should match number of active channels
- Propagation speeds should be physiologically plausible
- Consistent patterns across multiple events increase reliability