Differentiate between spikes and shar wave morphology differences

1. Spike
Duration: < 70 milliseconds
Shape: Very steep rise and fall, narrow and pointed
Appearance: Looks like a needle or a lightning bolt
Clinical Relevance: Often associated with epilepsy, especially focal epilepsy
Example Use: Seen in interictal epileptiform discharges (IEDs)

2. Sharp Wave
Duration: 70–200 milliseconds
Shape: Still pointed, but broader and less steep than a spike
Appearance: More like a hill with a sharp peak
Clinical Relevance: Also associated with epileptiform activity, but may be seen in slower seizure types or diffuse abnormalities

How to Tell the Difference
Here are some ways to distinguish between true seizure activity and artifact:

Feature	Seizure Activity	Artifact
Location	Can involve multiple regions, not just frontal	Mostly frontal (Fp1/Fp2)
Morphology	Sharp waves, spikes, rhythmic discharges	Irregular, slow waves or high-frequency bursts
Duration	Lasts seconds to minutes, often stereotyped	Brief, inconsistent
Clinical Correlation	Matches observed behavior (e.g., eye fluttering, loss of awareness)	No clinical signs or unrelated movements
Video-EEG	Shows eye fluttering as part of seizure	Shows blinking or movement unrelated to seizure

Notes from article https://iopscience.iop.org/article/10.1088/2057-1976/ad097f

Seizure Types:

 About seizure, IAS refers to focal onset impaired awareness, WIAS is focal onset without impaired awareness, and lastly, FBTC is focal to bilateral tonic-clonic. Localization refers to which brain lobe the seizure originates, here T is temporal and F is frontal. The lateralization refers to the side of origin in the brain, here R is right, L is left, and bilateral is both sides simultaneously. Other metrics are self-explanatory. In total, the database contains 47 seizure instances with a mean seizure duration of 61 seconds on approximately 144 hours of monitoring time. Figure 1 shows the seizures’ duration among patients.

 2.2. Automatic seizure detection
Seizure detection aims to classify an epoch as either seizure or non-seizure. In this study, the seizure detection involved pre-processing, features extraction from time domain, frequency domain and entropy and classification carried out with random forest. A schematic overview of the seizure detection architecture can be seen in figure 2.


2.2.2. Feature extraction
Extracting meaningful features is important for seizure detection. In order to deal with variability of patients with non-specific model, time domain features, frequency-domain features, and entropy derived features which reflect signal characteristics from different perspectives were extracted from every 6-seconds epoch of every channel (N = 19 channels). Those features have been found in literature and are widely used for adult scalp EEG [14]. In total, M = 22 features extracted from every channel were concatenated into a feature vector Xl with length of N · M = 418. Below, each feature is described along with their corresponding equations.

Time-domain features: The following features were extracted. Here, x represent EEG time series.

Number of zero crossings refers to the number of times the signal goes from a positive voltage value to a negative one or vice versa.
Maximum represents the largest voltage value.
Minimum represents the smallest voltage value.
Skewness is a measure of the lack of symmetry in the amplitude distribution. Larger values mean more skewness. It is computed as: 
 
, where u is the mean of x, σ is the standard deviation of x.
Kurtosis gives information about the extent of the peak in the amplitude distribution. It is computed as: 
 
, where u is the mean of x, σ is the standard deviation of x.
Root mean square measures the magnitude of the varying quantity [25] and is computed as in [26]: 
 
 
Mobility is the second of Hjorth’s three statistical time-domain parameters introduced by Bo Hjorth in 1970 [27]. The mobility parameter measures the standard deviation of the slope with reference to the amplitude’s standard deviation [27]. It is calculated as: 
 
 where 
 denotes the derivative of the signal x and σ the standard deviation.
Complexity is the third Hjorth’s parameter and gives an estimate of the bandwith of the signal indicating the similarity of the shape of the signal compared to a pure sine wave [28]. It is calculated as: 
 
 
 
 where 
 denotes the double derivative of the signal x, 
 denotes the derivative of the signal x and σ the standard deviation.

Frequency-domain features First, power spectral density (PSD) was calculated using the fast fourier transform (FFT) with the Welch’s method [29]. According to brain status and cognitive activities, the EEG can be classified into five different frequency sub-bands namely δ (0.5 − 4Hz); θ (4 − 8Hz); α (8 − 13Hz); β (13 − 30Hz); γ (30 − 80Hz). In the frequency domain, the following features were extracted where f represents the frequency and S(fi ) is the spectral power of that frequency:

Total power  
 
Peak frequency is the frequency with highest spectral power.  
 
Mean power in five frequency sub-bands δ, θ, α, β and high frequency (HF). Here nδ , nθ , nα , nβ , nHF represent the number of the samples in those sub-bands respectively. 
 
 
 
 
 
 
 
 
 
Normalized power in five frequency sub-bands δ, θ, α, β and high frequency 
 
 
 
 
 
 
 
 
 

Entropy features In general, entropy is an information measure that represents the amount of uncertainty associated with events from a given distribution [30, 31]. Two entropy measures were extracted namely sample entropy and the spectral entropy.

Sample entropy is proposed by Richman and Moorman in [32] and is used to assess the complexity of physiological time-series signals. It was computed as: 
 
 where x is a signal, m is the embedding dimension (m = 2 in the study), r is the radius of the neighbourhood (r = 0.2std(x) in the study), C(m + 1, r) is the number of embedded vectors of length m + 1 having a Chebyshec distance inferior to r and C(m, r) is the number of embedded vectors of length m having a Chebyshev distance inferior to r.
Spectral entropy is a measure of the degree of EEG irregularity because the entropy of the power spectrum represents the relative peakedness or flatness of the spectral distribution [31]. It is calculated as: 
 where P is the normalized PSD and fs is the sampling frequency.