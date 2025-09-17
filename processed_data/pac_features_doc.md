# Phase-Amplitude Coupling (PAC) Features Documentation

## Overview

Phase-Amplitude Coupling (PAC) quantifies the relationship between the phase of low-frequency oscillations and the amplitude of high-frequency oscillations. This cross-frequency coupling mechanism is thought to coordinate neural communication across different temporal and spatial scales, playing a crucial role in cognitive functions and pathological states.

## Theoretical Background

### Neural Basis
- Low-frequency oscillations provide temporal windows for neural processing
- High-frequency activity reflects local neuronal firing
- PAC represents hierarchical organization of brain rhythms
- Strong PAC indicates coordinated multi-scale neural dynamics

### Coupling Mechanism
- Phase of slow oscillations modulates excitability cycles
- Amplitude of fast oscillations varies with these excitability changes
- Results in nested oscillations where fast rhythms ride on slow waves

## Frequency Band Combinations

The function analyzes four specific PAC combinations:

### Phase Frequencies (Low)
- **Theta** (3.5-8 Hz): Memory, navigation, cognitive control
- **Alpha** (8-13 Hz): Attention, inhibitory control

### Amplitude Frequencies (High)
- **Gamma** (30-50 Hz): Local processing, feature binding
- **High Gamma** (50-80 Hz): Multi-unit activity proxy, local activation

### Coupling Pairs Analyzed
1. **Theta-Gamma PAC** (`pac_theta_gamma`)
   - Most studied coupling type
   - Associated with memory formation and retrieval
   - Increases during cognitive tasks

2. **Theta-High Gamma PAC** (`pac_theta_high_gamma`)
   - Reflects theta coordination of local neural firing
   - Strong during working memory tasks
   - Altered in psychiatric conditions

3. **Alpha-Gamma PAC** (`pac_alpha_gamma`)
   - Related to attention and sensory processing
   - Modulated by task demands
   - Inhibitory control mechanism

4. **Alpha-High Gamma PAC** (`pac_alpha_high_gamma`)
   - Links inhibitory alpha to local processing
   - Important for selective attention
   - Altered in disorders of consciousness

## Calculation Method

### Signal Processing Pipeline

1. **Phase Extraction**
   - Bandpass filter signal to phase frequency range
   - Apply Hilbert transform to get analytic signal
   - Extract instantaneous phase: angle(Hilbert(filtered_signal))
   - Phase values range from -π to π

2. **Amplitude Extraction**
   - Bandpass filter signal to amplitude frequency range
   - Apply Hilbert transform
   - Extract instantaneous amplitude: |Hilbert(filtered_signal)|
   - Represents envelope of high-frequency oscillations

### Modulation Index Calculation

1. **Phase Binning**
   - Divide phase range (-π to π) into 18 bins (20° each)
   - Ensures adequate sampling of phase cycle

2. **Amplitude Distribution**
   - For each phase bin, calculate mean amplitude
   - Creates phase-amplitude distribution
   - Formula: mean_amp[bin] = mean(amplitude[phase ∈ bin])

3. **Normalization**
   - Normalize amplitude distribution to sum to 1
   - Creates probability distribution: P = amp_by_phase / sum(amp_by_phase)

4. **Kullback-Leibler Divergence**
   - Compare to uniform distribution (no coupling)
   - KL divergence: D_KL = Σ P(i) × log(P(i) / U(i))
   - Where U = 1/n_bins (uniform distribution)

5. **Modulation Index**
   - Normalize KL divergence by log(n_bins)
   - MI = D_KL / log(n_bins)
   - Ranges from 0 (no coupling) to 1 (perfect coupling)

## Features Generated

### Individual Coupling Measures
**Features:** `pac_theta_gamma`, `pac_theta_high_gamma`, `pac_alpha_gamma`, `pac_alpha_high_gamma`

- **Value Range**: 0 to 1
- **Interpretation**:
  - 0: No phase-amplitude coupling
  - 0.01-0.03: Weak coupling (typical baseline)
  - 0.03-0.1: Moderate coupling
  - >0.1: Strong coupling (task-related or pathological)

### Aggregate Statistics
**Features:** `pac_mean`, `pac_max`

- **PAC Mean**: Average coupling strength across all frequency pairs
  - Overall cross-frequency coordination
  - Global measure of nested oscillations

- **PAC Max**: Maximum coupling strength among all pairs
  - Identifies dominant coupling mode
  - Sensitive to strong local coupling

## Clinical Applications

### Cognitive Function
- **Working Memory**: Increased theta-gamma PAC
- **Attention**: Modulated alpha-gamma PAC
- **Learning**: Enhanced during memory encoding
- **Executive Control**: Frontal theta-gamma coupling

### Neurological Disorders

#### Epilepsy
- **Interictal**: Abnormal PAC patterns in epileptogenic zones
- **Preictal**: Increased coupling before seizures
- **Ictal**: Disrupted normal PAC relationships
- **Localization**: Focal PAC abnormalities mark seizure onset zones

#### Parkinson's Disease
- Excessive beta-gamma PAC in motor cortex
- Reduced by dopaminergic medication
- Correlates with symptom severity

#### Alzheimer's Disease
- Decreased theta-gamma PAC
- Disrupted memory-related coupling
- Early biomarker potential

#### Schizophrenia
- Altered gamma coupling patterns
- Reduced task-related PAC modulation
- Associated with cognitive symptoms

## Interpretation Guidelines

### Normal Patterns
- **Task-dependent modulation**: PAC increases with cognitive load
- **Regional specificity**: Different regions show distinct PAC profiles
- **State dependence**: Varies with sleep/wake states
- **Age effects**: Changes across development and aging

### Pathological Indicators
- **Excessive coupling** (MI > 0.15): May indicate hypersynchrony
- **Absent coupling** (MI ≈ 0): Loss of cross-frequency coordination
- **Asymmetric patterns**: Unilateral abnormalities
- **Fixed coupling**: Lack of task-related modulation

### Quality Considerations
- **Signal quality**: PAC sensitive to artifacts
- **Segment length**: Needs sufficient cycles of slow frequency
- **Stationarity**: Assumes stable coupling during analysis window
- **Multiple comparisons**: Four coupling pairs tested

## Computational Considerations

### Optimization Strategies
- Limited to first 10 channels for computational efficiency
- Strategic selection of frequency pairs
- 18 phase bins balance resolution and reliability

### Processing Requirements
- Sufficient data length: >10 cycles of phase frequency
- Adequate sampling rate: >2× highest amplitude frequency
- Clean signal: Artifacts severely affect PAC estimates

### Validation Approaches
- Surrogate testing for statistical significance
- Phase shuffling to test null hypothesis
- Comparison with other PAC methods (PLV, MVL)

## Advantages of Modulation Index Method

### Strengths
- **Intuitive interpretation**: Normalized 0-1 scale
- **Robust to amplitude variations**: KL divergence approach
- **Well-validated**: Extensively used in literature
- **Computationally efficient**: No complex statistics required

### Limitations
- **Assumes stationarity**: Coupling stable over time window
- **Phase bin dependency**: Results vary with bin number
- **Single coupling value**: No frequency resolution within bands
- **No directionality**: Cannot determine causal direction

## Regional Analysis

PAC features can be calculated for specific brain regions:
- **Frontal PAC**: Executive function and control
- **Temporal PAC**: Memory processing
- **Parietal PAC**: Sensory integration
- **Occipital PAC**: Visual processing
- Features follow pattern: `{prefix}_{region}_pac_{phase}_{amplitude}`

## Relationship to Other Features

### Complementary to Power Analysis
- PAC reveals interactions invisible in power spectra
- Can occur without power changes
- Provides functional connectivity information

### Distinct from Coherence
- Coherence: Same-frequency synchronization
- PAC: Cross-frequency interaction
- Both important for network analysis

### Enhancement of Entropy Measures
- High PAC suggests organized (low entropy) coupling
- Adds directional information to complexity measures

## Future Directions

### Advanced Methods
- Time-varying PAC for dynamic analysis
- Multiple frequency pairs simultaneously
- Directional PAC using information theory
- Source-level PAC after solving inverse problem

### Clinical Translation
- Real-time PAC for neuromodulation
- Biomarker development for early diagnosis
- Treatment monitoring and optimization
- Closed-loop therapeutic systems