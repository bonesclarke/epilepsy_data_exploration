# epilepsy_data_exploration

Potential research areas:

"Age-Related Seizure Network Maturation and Implications for Gene Therapy Timing Windows"

The Research Question
"Do focal seizure propagation patterns differ systematically between pediatric and adult patients, and could these differences inform optimal timing windows for gene therapy interventions?"

Why This Matters for Gene Therapy
Research has shown that genetic interventions may have temporally narrow windows of efficacy, after (or before) which little amelioration of the disease phenotype may be seen. This may be especially important for therapies targeting ion channels expressed only transiently in development Are Genetic Therapies for Epilepsy Ready for the Clinic? - James S. Street, Yichen Qiu, Gabriele Lignani, 2023.
Your combined dataset gives you a unique advantage because:

CHB-MIT: Pediatric focal epilepsy (developing brains)
Siena: Adult focal epilepsy (mature brains)
Gene therapy relevance: Understanding when brain networks are most modifiable

What You'd Analyze (Reproving Known Concepts)

Seizure Propagation Speed: Compare how quickly seizures spread across brain regions in children vs adults
Network Connectivity Patterns: Analyze which brain regions are most involved in seizure networks at different ages
Pre-ictal Signal Characteristics: Look for age-related differences in the lead-up to seizures


First problem to solve: How quickly do seizures spread across brain regions?


Second problem to solve: What brain regions are most involved in seizure networks?


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
