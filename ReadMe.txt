prep_eeg_check_trigger : 
This script focuses on verifying that the data structure and metadata match the expected recording setup.
Interactive visualization helps ensure that the events in the data align with experimental expectations.

Make sure the .bdf file is in the specified path ("C:/Users/mfbpe/Desktop/DATA/2023_Eva_Freedom/raw\\Pa1.bdf").
Install the required library: mne.
Run the script in a Python environment compatible with MNE and matplotlib.

Summary of the raw data, including the number of channels, sampling rate, and recording duration.
List of channel names for verification.
Detected events printed in the console and visualized in an interactive plot.
Confirmation that the EEG data is accessible and loaded for further processing.

---------------------------------------------------

prep_eeg_bis :

This Python script processes raw EEG data files in .bdf (Biosemi format), resamples them, identifies and cleans event markers, and exports the processed data to the BIDS (Brain Imaging Data Structure) format. The BIDS format is widely used for organizing and sharing neuroimaging data.

Key Functionalities:

Input and Output Paths:
The script takes EEG data from a source directory (root_source) and saves the processed data into a BIDS-compliant directory structure (root_bids).

File Handling:
The script reads all .bdf files from the source directory and processes each one individually. For each file:

It extracts the participant number from the filename.
Loads and processes the raw EEG data.

Event Handling:
The script detects EEG event markers, then crops the EEG data around those events. After that, it resamples the data to reduce its size and computational load. It also filters the events to keep only relevant ones, based on a predefined dictionary (event_id).

EEG Data Processing:

The EEG data is resampled to 1/4th of its original sampling rate to reduce the data size and make the processing faster.
It applies a standard EEG montage (BioSemi 64), ensuring that the channel positions are consistent with standard neuroscience conventions.
Output Format:
After processing, the EEG data is written into the BIDS format, specifically using the BrainVision file format, which is compatible with many EEG analysis software packages.

How to Run:

Ensure that your raw EEG data files are stored in the directory specified by root_source.
The output will be saved in the folder specified by root_bids, organized according to the BIDS format.
Install the required libraries: mne, mne_bids, glob, and re.
Execute the script to convert and process the EEG data into BIDS format.
Important Considerations:

The script assumes that all raw data files follow a specific naming convention that includes a participant identifier (e.g., 'Pa001.bdf'). Adjustments may be needed if the file naming convention differs.
Event cropping and resampling are key steps to prepare the data for further analysis.
Make sure the system has sufficient resources to handle large EEG datasets, as the operations (resampling, event handling) may require substantial memory.


--------------------------------------------

prep_eeg_semi-auto_stage1 :
 
This script automates the detection and correction of noisy EEG channels and ICA artifact removal, but manual inspection of the ICA components is recommended to ensure accurate artifact rejection. It generates visualizations of ICA components and logs the bad channels and ICA components for each participant.


Key Functionalities:

A bandpass filter is applied (0.01 Hz to 40 Hz) to remove slow drifts and high-frequency noise.

Handling Bad Channels:

For each participant, bad (noisy) channels are either interpolated (if known in advance) or automatically detected using the NoisyChannels class.
Detected bad channels are interpolated to minimize their impact on further analyses.

Independent Component Analysis (ICA):

ICA is applied to the EEG data to detect and remove artifacts like eye blinks or muscle movements.
If pre-determined bad ICA components are known, they are excluded from the data. Otherwise, ICA is performed and the components are plotted and saved for manual inspection.

Saving Processed Data:

The cleaned and processed EEG data is saved in .fif format.
The script also logs the bad channels and bad ICA components in separate text files (list_bad_channels.txt and list_bad_ica.txt).

Visualizing ICA Components:

The script generates images of the ICA components for each participant and saves them in the specified directory for later inspection.

How to Run:

Ensure the EEG data is stored in BIDS format and located in the directory specified by root_bids.
Install the required Python libraries: mne, pyprep, glob, and matplotlib.

The script will automatically process each participant's EEG data, save the cleaned data, generate ICA component plots, and store logs of bad channels and ICA components.

Key Outputs:

Preprocessed EEG data in .fif format for each participant.
ICA component images saved in the directory specified by root_image.
Text files (list_bad_channels.txt and list_bad_ica.txt) containing bad channels and ICA components for each participant.

--------------------------------------------

prep_eeg_semi-auto_stage2 :

The script allows for manual corrections of EEG data (e.g., marking bad channels and bad segments), which should be reviewed before proceeding with automatic processing.
ICA component plots should be reviewed to ensure correct identification of bad components. This script is also made to remove bad ICA components based on visual inspection from the figure of stage 1. 

Key Functionalities:

Visual Inspection:

The script allows manual inspection and annotation of the raw EEG data using raw.plot() with interactive plotting enabled.

Handling Bad Channels:

Known bad channels for participants are interpolated. If unknown, the script automatically detects noisy channels using the NoisyChannels class from the pyprep package.

Independent Component Analysis (ICA):

ICA is performed to remove artifacts (e.g., eye blinks). Predefined bad ICA components are excluded for each participant, or new components can be identified.
ICA components are visualized and saved for review.
Saving Processed Data:

The preprocessed EEG data is saved in .fif format.
Bad channels and ICA components are logged into two text files: list_bad_channels.txt and list_bad_ica.txt.



post_eeg_ERP :

This code provides a comprehensive workflow for processing EEG data to study ERP (with the case of the readiness potential) under different  conditions. By cleaning the data, removing artifacts, and extracting meaningful metrics, it sets the foundation for subsequent statistical analyses and interpretations of EEG.

Data Loading and Preprocessing:

Loads raw EEG data files in .fif format for multiple participants.
Sets the EEG reference using the Reference Electrode Standardization Technique (REST).
Identifies events corresponding to different action conditions (here : free, recommended, semi-instructed, instructed).
Creates epochs around these events and applies baseline correction.

Trial Rejection:

Implements a custom function drop_trials to identify and remove trials that are outliers based on peak-to-peak amplitude, mean amplitude, and slope within a specified time window.
Uses statistical measures (interquartile range) to determine outlier thresholds.

Data Saving and Cleaning:

Saves the cleaned epochs to new files for further analysis.
Logs statistics about the trials that were dropped.

Grand Averaging and Visualization:

Computes the grand average of the readiness potentials across participants, excluding any specified participants.
Plots the grand average RP to visualize the overall trend.

Condition-wise Analysis:

Separates the data based on the different action conditions.
Computes the average evoked responses for each condition.
Plots the grand averages for each condition to compare the effects of different instructions.

Data Extraction for Statistical Analysis:

Calculates the mean and slope of the RP for each trial within a specified time window (-1s to 0s before the event).

Writes this data to a CSV file (data_RP_lmer.csv) for further statistical analysis, such as linear mixed-effects modeling.
How to Use the Code:


Review Outputs:

The script will generate cleaned EEG epochs saved as .fif files.
It will produce plots of the grand average RP and condition-wise comparisons.
A CSV file (data_RP_lmer.csv) will be created containing the extracted RP metrics for statistical analysis.

Adjustments:

Modify the bad_prt list to exclude any participants that should not be included in the analysis.
Change the time windows (T1, T2) in the drop_trials function to adjust the period over which outlier detection is performed.
Update the plotting styles and colors as desired.
Key Functions and Parameters:

drop_trials(epochs, chs, do_peak, do_mean, do_slope, T1, T2): Custom function to reject trials based on specified criteria.

epochs: MNE Epochs object containing the EEG data.
chs: Number of channels to analyze.
do_peak, do_mean, do_slope: Flags to enable rejection based on peak-to-peak amplitude, mean amplitude, and slope.
T1, T2: Start and end times for the analysis window.
event_action: Dictionary mapping action condition labels to event IDs.

RP_all: List containing the cleaned epochs for each participant.

evokeds_RP_all: Dictionary storing the average evoked responses for each condition.








