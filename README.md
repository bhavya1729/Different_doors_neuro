# Different_doors_neuro
The project aims to analyze Diiferet doors experimental data set (a 40-participant EEG dataset) and identify whether brain generates stronger error signals when feedback is informative and learnable compared to when it is random and unlearnable.
### Data source: 
[Diiferet_doors experimental data set](https://openneuro.org/datasets/ds007647/versions/1.0.1) by Abigail Oloriz and Cameron D. Hassall (2026). Different Doors. OpenNeuro. [Dataset] doi: doi:10.18112/openneuro.ds007647.v1.0.1

Experiment: Forty participants selected one of two doors, then received feedback indicating either a monetary gain or loss. Blocks lasted 20 trials and, unbeknownst to participants, were of two types. In learnable blocks, one of the doors was better because choosing it was associated with a 60% likelihood of a win. The other door only paid out 10% of the time. In unlearnable blocks, outcomes were not yoked to participant actions but were instead drawn from the learnable blocks and presented in random order. Thus, the win and loss totals were matched across the block types. There were 20 blocks in total (10 of each type), and each block was followed by a short survey asking which door was better, and whether the participant had fun, felt motivated, and did well.

## Behavioral Results Summary
### methodology: 
All 40 participants' trial-by-trial behavioural data was loaded from BIDS-formatted .tsv files and combined into a single analysis DataFrame. Four analyses were run.

#### 1. Learning Performance
At the beginning of the task, participants in both conditions perform at chance level (around 0.5 optimal choice rate), meaning they are initially guessing.
Learnable condition:
Participants gradually improve over time and increasingly choose the optimal option.
Unlearnable condition:
Performance remains around chance level (~0.5), showing no systematic improvement.
Interpretation
This indicates that participants successfully learn the structure of the learnable condition, while no stable learning is possible in the unlearnable condition.

#### 2. Reaction Time (Decision Speed)
Reaction times were compared across conditions and across trials.
Both learnable and unlearnable conditions show similar reaction times.
No meaningful difference in overall decision speed is observed between conditions.
Reaction time does not strongly change across trials.
Interpretation
Although participants learn which option is better in the learnable condition, this learning does not translate into faster decision-making. Decisions remain similarly fast in both conditions.

#### 3. Win Stay or Lose Shift Behavior
We examined how previous outcomes influence future choices.
Win Stay Behavior
Learnable condition: ~0.80
Unlearnable condition: ~0.70
Participants are more likely to repeat a choice after receiving a reward, especially in the learnable condition.
Lose Shift Behavior
Both conditions: ~0.47
loss is less likely to influence the choice
Interpretation
Behavior is more strongly influenced by wins than losses. This suggests that participants rely more on reward reinforcement than on punishment when adjusting their decisions.

#### 4. Stay Bias Analysis
From the previous WSLS analysis, it was observed that the Lose–Shift rate was relatively low (~0.47) in both conditions. Participants therefore did not consistently switch choices after receiving a loss. One possible explanation is that participants had a general tendency to repeat their previous choice (stay bias), even when negative feedback suggested they should switch.
Thus For each participant, the proportion of trials on which they repeated their previous response ("stay rate") was calculated.
A one-sample t-test was then used to compare the average stay rate across participants against a chance level of 50% (0.5).
Null Hypothesis:
The average stay rate is equal to 0.5, meaning participants have no preference for staying or switching.
Results
The mean stay rate was 0.637 (63.7%), indicating that participants repeated their previous choice on nearly two-thirds of trials.
A one-sample t-test showed that this stay rate was significantly greater than the chance level of 50% (p < 0.001).
Interpretation:
Participants exhibited a clear stay bias, meaning they were more likely to repeat their previous choice than switch to an alternative option.
This finding provides a possible explanation for the relatively low Lose Shift rates observed in the WSLS analysis. Even after receiving a loss, participants often continued selecting the same option rather than switching, suggesting the presence of choice perseveration or habit-like behavior during the task.

### Inference: The behavioural analysis shows that people do learn in learnable blocks, but imperfectly. They are slowed down by a stay bias that causes them to miss update opportunities.

Sub-01 had 4 bad channels, 4 ICA components removed (blinks), and 100% of epochs survived rejection. The PSD shows clean 1/f slope with no 60 Hz contamination. Data quality is acceptable. 


## EEG Preprocessing

### Methodology

The EEG preprocessing pipeline was first tested on Sub-01 to verify that all processing steps were functioning correctly and to identify any necessary adjustments before analysing the full dataset.

Raw BrainVision EEG recordings were imported into MNE-Python. Sampling rates were read directly from each file header to ensure accurate timing information. A 60 Hz notch filter was applied to remove power-line noise, followed by a 0.1–40 Hz bandpass filter to reduce slow drifts and high-frequency muscle activity while preserving ERP components of interest.

Bad channels were identified using a combination of automated detection and visual inspection and were reconstructed through interpolation from neighbouring electrodes. The data were then re-referenced to the average of all electrodes.

Independent Component Analysis (ICA) was used to separate neural activity from artefacts. Components representing eye blinks, eye movements, or muscle activity were removed only when supported by inspection of their scalp topography, time course, and EOG correlation.

The cleaned continuous data were segmented into epochs extending from −200 ms to +800 ms relative to feedback onset. Baseline correction was applied using the pre-feedback interval. Epochs exceeding ±150 μV were rejected as residual artefacts.

For each participant, a preprocessing summary was generated documenting bad channels, removed ICA components, and epoch retention.

### Data Quality Verification
Sub-01 was first used as a quality-control participant to validate the preprocessing pipeline. Four bad channels were identified and interpolated, four blink-related ICA components were removed, and all epochs survived artefact rejection. The power spectral density displayed a typical 1/f profile with no notable 60 Hz contamination, indicating satisfactory signal quality. Following successful 

## ERP and FRN Analysis
### Methodology
Event-Related Potentials (ERPs) were computed separately for each participant and condition before being averaged across participants, ensuring equal weighting of all individuals regardless of trial count.
Grand-average ERPs were generated for win and loss feedback in both learnable and unlearnable conditions. Difference waves (loss minus win) were then calculated to isolate feedback-related activity.
The Feedback-Related Negativity (FRN) was quantified at electrode FCz as the mean voltage between 200–350 ms following feedback presentation. FRN amplitude was calculated as the difference between loss and win responses. A paired-samples t-test compared FRN amplitudes between learnable and unlearnable conditions across participants.
### Results
FRN being larger in learnable than unlearnable blocks was not supported.
Learnable FRN: Mean = -0.416 µV (SD = 1.323)
Unlearnable FRN: Mean = -0.568 µV (SD = 1.349)
t(39) = 0.734
p = 0.467
Cohen's d = 0.114
The FRN did not differ significantly between conditions. Although the unlearnable condition produced a slightly more negative FRN, the effect was very small and not statistically reliable.

##### Note:
Since this analysis was done independently, the possibility that some mistake in preprocessing, FRN measurement or analysis contributed to the null result cannot be ruled out
Another question is whether participants may have treated learnable and unlearnable blocks similarly. Since they were never explicitly told which type of block they were in, is it possible that they continued searching for patterns and learning opportunities in both conditions? 
