# Dataset: Digital Twin Predictive Maintenance

## Publication

This repository hosts the complete experimental data supporting the following research:

**"Cross-domain digital twin architecture for predictive maintenance via machine learning and Large Language Models"**

Authors: John Williams, Gerald Jones, Tom Berg, Luke Birt, Ashley Stowe, and Xueping Li

Published: *Computers and Industrial Engineering*, Vol. 215, No. 111914 (2026)

DOI: https://doi.org/10.1016/j.cie.2026.111914

Data source: University of Tennessee, Knoxville Advanced Systems Lab (UTK-ASL) - https://asl.utk.edu

---

## Overview

This dataset contains comprehensive vibration and temperature monitoring data collected from a test bed motor under both baseline (healthy) and faulty operating conditions. The data was captured using two different motors of the same model, and two different monitoring platforms—**Bently Nevada System1** and **Cutsforth InsightCM**—enabling cross-platform validation and comparative analysis for predictive maintenance model development.

### Key Features

- **Dual Monitoring Platforms**: Same equipment monitored via two independent systems for platform comparison and fusion studies
- **Multiple Fault Types**: 5 distinct fault conditions plus 2 baseline reference states
- **High-Resolution Data**: Sub-minute to 1-second sampling intervals with dense sensor coverage
- **JSON + CSV Representations**: InsightCM measurements are available as JSON event records and tabular CSV summaries
- **Git-Friendly Structure**: Large JSON files split into 10 MB chunks for easy distribution

### Intended Uses

- ✅ **Experiment Reproduction**: Recreate the cross-domain digital twin architecture from the paper
- ✅ **Comparative Platform Analysis**: Validate sensor fusion approaches across monitoring systems
- ✅ **Fault Detection Model Development**: Train ML classifiers on real industrial fault data
- ✅ **Condition Monitoring**: Establish baselines and anomaly detection thresholds
- ✅ **Digital Twin Validation**: Calibrate physics-based models with measured equipment behavior
- ✅ **Streaming Analytics**: Prototype real-time IoT data processing pipelines
- ✅ **Alternative Applications**: Extend the data for different fault types or analysis methods

---

## Repository Structure

```
Dataset_digital_twin_predictive_maintenance/
│
├── README.md                           (this file)
│
├── data/
│   │
│   ├── BentlyNevada_System1/           (Bently Nevada System1 platform data)
│   │   ├── README.md                   (Complete documentation)
│   │   ├── s1_testbed_data.csv         (Testbed configuration summary)
│   │   ├── CombinedFaults_02222025.csv (Master file: all faults combined)
│   │   │
│   │   ├── 01232025_Baseline/          (Baseline state, first measurement)
│   │   ├── 01302025_Baseline/          (Baseline state, second measurement)
│   │   ├── 01222025_EccentricRotor/    (Eccentric rotor fault)
│   │   ├── 02052025_BentShaft/         (Bent shaft fault)
│   │   ├── 02132025_FaultedBearing/    (Bearing fault)
│   │   ├── 02172025_FaultedCoupling/   (Coupling misalignment)
│   │   ├── 02172025_Imbalance/         (Rotor imbalance)
│   │   │
│   │   └── CombinedFiles/              (Pre-aggregated by fault type)
│   │       ├── Combined_B1.csv         (Baseline 1)
│   │       ├── Combined_B2.csv         (Baseline 2)
│   │       ├── Combined_EC.csv         (Eccentric rotor)
│   │       ├── Combined_FB.csv         (Bearing fault)
│   │       ├── Combined_FC.csv         (Coupling fault)
│   │       ├── Combined_BS.csv         (Bent shaft)
│   │       └── Combined_IM.csv         (Imbalance)
│   │
│   └── Cutsforth_InsightCM/            (Cutsforth InsightCM platform data)
│       ├── README.md                   (Complete documentation)
│       ├── final.csv                   (Recommended: Processed summary data)
│       ├── icm_chiller.json            (InsightCM measurements in JSON format, 244 MB)
│       │
│       ├── icm_chiller_split_10mb/     (RECOMMENDED: Split into 10 MB chunks)
│       │   ├── icm_chiller_part001.json
│       │   ├── icm_chiller_part002.json
│       │   └── ... (24 files total)
│       │
│       └── image/                      (Dataset images/figures)
│
├── paper/
│   ├── 1-s2.0-S0360835226001154-main.pdf (Published paper PDF)
│   └── readme.md                         (Paper directory notes)
│
└── LICENSE                               (MIT License)
```

---

## Quick Start Guide

### For Rapid Exploration

**Start here if you want to quickly prototype models:**

```bash
# Download or clone the repository
git clone https://github.com/UTK-ASL/Dataset_digital_twin_predictive_maintenance.git

# Navigate to data directory
cd Dataset_digital_twin_predictive_maintenance/data

# Python: Load and explore System1 combined data
python
>>> import pandas as pd
>>> system1_faults = pd.read_csv('BentlyNevada_System1/CombinedFaults_02222025.csv', parse_dates=['Timestamp'])
>>> print(system1_faults.head())
>>> print(system1_faults.describe())

# Python: Load and explore InsightCM processed data
>>> insight_data = pd.read_csv('Cutsforth_InsightCM/final.csv', parse_dates=['datetime'])
>>> print(insight_data.head())
```

### For JSON Data / High-Resolution Analysis

**Use these approaches for working with split JSON files:**

```python
# Load all InsightCM split files
import json
import pandas as pd

records = []
for i in range(1, 25):  # 24 split files
    filename = f'Cutsforth_InsightCM/icm_chiller_split_10mb/icm_chiller_part{i:03d}.json'
    with open(filename, 'r') as f:
        records.extend(json.load(f))

df = pd.DataFrame(records)
print(f"Loaded {len(df)} sensor records")
```

### For Reproducing the Paper's Experiments

See the **[Experiment Reproduction](#experiment-reproduction)** section below.

---

## Data Overview

### Monitoring Platforms Comparison

| Aspect                   | Bently Nevada System1       | Cutsforth InsightCM                             |
| ------------------------ | --------------------------- | ----------------------------------------------- |
| **Format**         | CSV files by channel/metric | JSON event records + CSV summary                |
| **Channels**       | 2 accelerometers            | 4 accelerometers, 4 thermocouples, 1 tachometer |
| **Sampling**       | ~1 minute intervals         | ~1-5 second intervals                           |
| **Total Records**  | ~15,000                     | 852,860                                         |
| **Faults Covered** | Multiple fault types        | Multiple operating states (status labels)       |
| **File Size**      | ~5 MB total                 | ~244 MB (JSON), 1.1 MB (CSV)                    |
| **Best For**       | Multi-fault classification  | Real-time streaming, dense time-series          |

### Operating States

Each dataset includes measurements from these motor operating conditions:

| State Category                 | System1      | InsightCM                            | Description                                 |
| ------------------------------ | ------------ | ------------------------------------ | ------------------------------------------- |
| **Baseline (Healthy)**   | ✓ (2 meas.) | ✓                                   | Reference state for normal operation        |
| **Faulted Operation**    | ✓           | ✓ (indicated by `status` labels)  | Non-baseline operating conditions           |
| **State Labeling Style** | Folder-based | Row-level labels in JSON/CSV records | Use labels for filtering and model training |

### Data Characteristics

**Bently Nevada System1**:

- 7 operating states (2 baseline + 5 faults)
- 2 measurement channels
- 6 metrics per channel (bias, derived peak, direct, RMS, velocity peak, velocity RMS)
- ~13-14 files per state
- Individual CSV file per metric or combined per state

**Cutsforth InsightCM**:

- 852,860 discrete sensor readings
- Multi-sensor integration (temperature + vibration + speed)
- 6 derived metrics per accelerometer
- Sub-minute temporal resolution
- JSON event records + aggregated CSV summary

---

## Experiment Reproduction

### Requirements

- Python 3.8+
- Pandas, NumPy, Scikit-learn
- TensorFlow/PyTorch (for deep learning models)
- Matplotlib, Seaborn (visualization)

### Dataset Preparation

1. **Clone or download the repository**:

   ```bash
   git clone https://github.com/UTK-ASL/Dataset_digital_twin_predictive_maintenance.git
   cd Dataset_digital_twin_predictive_maintenance/data
   ```
2. **For System1 experiments**, use the aggregated CSV files:

   ```python
   import pandas as pd
   df = pd.read_csv('BentlyNevada_System1/CombinedFaults_02222025.csv', parse_dates=['Timestamp'])
   ```
3. **For InsightCM experiments**, load processed data:

   ```python
   df = pd.read_csv('Cutsforth_InsightCM/final.csv', parse_dates=['datetime'])
   ```
4. **For multi-platform fusion**, combine both datasets:

   ```python
   system1 = pd.read_csv('BentlyNevada_System1/CombinedFaults_02222025.csv', parse_dates=['Timestamp'])
   insight = pd.read_csv('Cutsforth_InsightCM/final.csv', parse_dates=['datetime'])

   # Align timestamps and synchronize measurements
   # (Implementation in paper's methodology section)
   ```

### Feature Engineering (from Paper)

Key features used in the original research:

**Time-Domain Features**:

- Mean, standard deviation, min, max per window
- Skewness, kurtosis
- Crest factor (peak/RMS)
- Peak-to-peak values

**Multi-Sensor Fusion**:

- Cross-correlation between channels
- Temperature-vibration coupling effects
- Multi-axis vibration vector magnitude

### Model Training Baseline

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split

# Load data
df = pd.read_csv('BentlyNevada_System1/CombinedFaults_02222025.csv', parse_dates=['Timestamp'])

# Prepare features and labels
X = df.drop(['Timestamp'], axis=1)
y = df['State']  # or appropriate label column

# Split and scale
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Train baseline model
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train_scaled, y_train)

# Evaluate
accuracy = model.score(X_test_scaled, y_test)
print(f"Baseline accuracy: {accuracy:.4f}")
```

### For the Full Cross-Domain Digital Twin Architecture

See the paper's implementation details and methodology section for:

- Large Language Model integration for anomaly interpretation
- Digital twin model validation procedures
- Cross-domain knowledge transfer techniques
- Real-time monitoring pipeline architecture

---

## Data Reconstruction and File Preparation

### Combining Split JSON Files (InsightCM)

If you need to work with the complete uncompressed JSON:

```python
import json
from pathlib import Path

output_file = 'icm_chiller_reconstructed.json'
records = []

# Load all split files
for i in range(1, 25):
    part_file = f'Cutsforth_InsightCM/icm_chiller_split_10mb/icm_chiller_part{i:03d}.json'
    with open(part_file, 'r') as f:
        records.extend(json.load(f))

# Write combined file
with open(output_file, 'w') as f:
    json.dump(records, f)

print(f"Reconstructed {len(records)} records to {output_file}")
```

### Working with CSV Aggregations (Recommended)

For most analysis, use the pre-aggregated CSV files which are already cleaned and aligned:

```python
# System1: Multi-fault consolidated data
system1_data = pd.read_csv('BentlyNevada_System1/CombinedFaults_02222025.csv')

# InsightCM: Processed sensor fusion
insight_data = pd.read_csv('Cutsforth_InsightCM/final.csv')
```

---

## Data Usage Guidelines

### ✅ Recommended Approaches

- **Rapid Prototyping**: Use `CombinedFaults_02222025.csv` and `final.csv`
- **Multi-Platform Fusion**: Combine both datasets for cross-validation studies
- **Baseline Establishment**: Use baseline folders for threshold calculation
- **Fault Signature Analysis**: Compare metric distributions across fault types
- **Time-Series Modeling**: Use InsightCM data for LSTM/GRU approaches

### ⚠️ Important Notes

- **Baseline Variance**: Two baseline measurements provided for reference validation
- **Platform Differences**: Do not directly compare values between System1 and InsightCM without normalization
- **Temporal Alignment**: Timestamps differ between platforms; careful synchronization needed for fusion

### ❌ Limitations to Be Aware Of

- Single motor test bed (may not generalize to other equipment)
- Relatively short measurement duration per state
- InsightCM state labels should be validated for each analysis objective
- No environmental factors or external load variations documented

---

## Citation

If you use this dataset in your research, please cite:

**BibTeX**:

```bibtex
@article{Williams2026CrossDomain,
  author = {Williams, John and Jones, Gerald and Berg, Tom and Birt, Luke and Stowe, Ashley and Li, Xueping},
  title = {Cross-domain digital twin architecture for predictive maintenance via machine learning and Large Language Models},
  journal = {Computers and Industrial Engineering},
  volume = {215},
  number = {111914},
  year = {2026},
  doi = {10.1016/j.cie.2026.111914}
}
```

**APA**:

> Williams, J., Jones, G., Berg, T., Birt, L., Stowe, A., & Li, X. (2026). Cross-domain digital twin architecture for predictive maintenance via machine learning and Large Language Models. *Computers and Industrial Engineering*, 215, 111914. https://doi.org/10.1016/j.cie.2026.111914

---

## Documentation

For detailed information about each dataset:

- **[BentlyNevada_System1/README.md](data/BentlyNevada_System1/README.md)**: Complete documentation for System1 platform

  - File structure and operating states
  - Measurement metrics and units
  - Fault condition descriptions
  - Feature engineering suggestions
  - Analysis workflows
- **[Cutsforth_InsightCM/README.md](data/Cutsforth_InsightCM/README.md)**: Complete documentation for InsightCM platform

  - JSON record format and structure
  - Processed CSV aggregation
  - Sensor specifications
  - High-resolution analysis approaches
  - Comparison with System1 data

---

## Contributing

This is a research dataset supporting a published paper. For questions or issues:

1. **Data Issues**: If you discover data quality problems, please open an issue with details
2. **Usage Questions**: Check the detailed READMEs first
3. **Extensions**: New analysis results are welcome; consider submitting as a paper citation

---

## License

This dataset is provided under the **MIT License**. See [LICENSE](LICENSE) file for details.

### License Summary

- ✅ **Allowed**: Use for research, commercial applications, modifications
- ✅ **Required**: Include license notice and attribution
- ✅ **Recommended**: Cite the original paper in your work

---

## Acknowledgments

Data collection and curation were led by the University of Tennessee, Knoxville Advanced Systems Lab (UTK-ASL).

Learn more about the lab: https://asl.utk.edu

Equipment: Test bed motor monitoring via Bently Nevada System1 and Cutsforth InsightCM platforms.

---

## Contact & Support

For questions about:

- **The Dataset**: See the detailed READMEs in each data directory
- **The Paper**: Refer to the published article (DOI: https://doi.org/10.1016/j.cie.2026.111914)
  PDF available in the *'paper'* directory
- **This Repository**: Open an issue on GitHub

---

## Version History

- **v1.0** (May 2026): Initial release supporting paper publication
  - Complete System1 multi-fault dataset
  - Complete InsightCM high-resolution dataset
  - Split JSON files for Git distribution
  - Comprehensive documentation
