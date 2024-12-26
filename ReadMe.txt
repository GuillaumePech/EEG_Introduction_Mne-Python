# EEG Analysis Scripts

## Overview
These scripts demonstrate various steps in EEG studies. They were created across different studies and have been used as teaching materials:

- **`prep_eeg_check_trigger`**: Verifies EEG data structure after the first pilot.
- **`prep_eeg_bids`**: Converts data to BIDS format ([Gorgolewski et al., 2016](https://doi.org/10.1038/sdata.2016.44)).
- **`prep_eeg_semi-auto_stage1`**: Detects bad channels, performs ICA, and saves ICA component figures.
- **`prep_eeg_semi-auto_stage2`**: Removes bad ICA components and allows for manual annotation.
- **`post_eeg_ERP`**: Processes EEG data for ERP analysis.
- **`post_eeg_TFR`**: Conducts time-frequency analysis of EEG data.

---

## Script Descriptions

### `prep_eeg_check_trigger`
Verifies the data structure and metadata alignment with the experimental setup.

**Key Features:**
- Interactive visualization to ensure event alignment.
- Summarizes raw data (channels, sampling rate, duration).
- Lists channel names and visualizes detected events.

**Usage:**
- Ensure the `.bdf` file is in the specified path (e.g., `C:/Users/.../raw/Pa1.bdf`).
- Install the required library: `mne`.
- Run the script in a Python environment compatible with MNE and matplotlib.

---

### `prep_eeg_bids`
Processes raw EEG data (.bdf format) into BIDS-compliant structure.

**Key Features:**
- Resamples EEG data and cleans event markers.
- Exports data in BrainVision file format.

**How to Run:**
1. Store raw EEG files in the specified `root_source` directory.
2. Install required libraries: `mne`, `mne_bids`, `glob`, `re`.
3. Execute the script to process and export data.

**Considerations:**
- Adjust for participant naming conventions (e.g., `Pa001.bdf`).
- Event cropping and resampling are essential for analysis readiness.

---

### `prep_eeg_semi-auto_stage1`
Automates noisy channel detection and ICA artifact removal.

**Key Features:**
- Bandpass filter: 0.01 Hz to 40 Hz.
- Automatically detects and interpolates bad channels.
- Performs ICA and saves components for review.

**Outputs:**
- Preprocessed data in `.fif` format.
- Logs bad channels and ICA components.
- Generates ICA component plots for inspection.

**Usage:**
- Ensure data is in BIDS format.
- Install required libraries: `mne`, `pyprep`, `glob`, `matplotlib`.

---

### `prep_eeg_semi-auto_stage2`
Enables manual corrections and finalizes ICA-based artifact removal.

**Key Features:**
- Manual inspection of EEG data using `raw.plot()`.
- Allows marking of bad channels and segments.
- Logs bad channels and ICA components.

**Usage:**
- Visualize and annotate ICA component plots.
- Save cleaned data for further processing.

---

### `post_eeg_ERP`
Processes EEG data for ERP analysis.

**Key Features:**
- Epochs creation and baseline correction.
- Trial rejection based on amplitude and slope criteria.
- Grand averaging and condition-wise analysis.

**Outputs:**
- Cleaned epochs in `.fif` format.
- CSV file (`data_RP_lmer.csv`) for statistical modeling.

---

### `post_eeg_TFR`
Analyzes EEG data in the time-frequency domain.

**Key Features:**
- Computes TFR using Morlet wavelets (2-30 Hz).
- Implements baseline correction.
- Extracts frontal-midline theta (FMtheta) power.

**Outputs:**
- Grand averages of TFR data.
- Time-frequency plots with statistical masks.
- CSV file with extracted features for statistical modeling.

---

**For more detailed information, refer to the respective script comments or documentation.**

