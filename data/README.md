# Dataset Documentation

## Dataset Used

This project uses the **MIT-BIH Arrhythmia Database**, a publicly available ECG dataset from PhysioNet.

The dataset contains annotated ECG recordings that are widely used for arrhythmia classification, ECG beat detection, and ECG anomaly detection research.

## Dataset Source

The dataset was downloaded from PhysioNet:

https://physionet.org/content/mitdb/1.0.0/

## Current Data Location

In this repository, the dataset is stored in:

```text
data/raw/mitdb/1.0.0/
```

The current folder structure is:

```text
ecg-anomaly-detection/
└── data/
    └── raw/
        └── mitdb/
            └── 1.0.0/
                ├── 100.dat
                ├── 100.hea
                ├── 100.atr
                ├── 101.dat
                ├── 101.hea
                ├── 101.atr
                ├── RECORDS
                ├── ANNOTATORS
                ├── SHA256SUMS.txt
                └── ...
```

## File Description

Each ECG record is made up of several related files. For example, record `100` contains:

```text
100.dat
100.hea
100.atr
100.xws
```

The most important files are:

| File Type | Description                                                                                                  |
| --------- | ------------------------------------------------------------------------------------------------------------ |
| `.dat`    | Contains the raw ECG signal data                                                                             |
| `.hea`    | Header file containing information such as sampling frequency, signal format, lead names, and record details |
| `.atr`    | Annotation file containing beat locations and beat labels                                                    |
| `.xws`    | Waveform display settings file used by PhysioNet/WFDB tools                                                  |

For most Python-based analysis, the main files needed are:

```text
.dat
.hea
.atr
```

## Example Record

For record `100`:

```text
100.dat   → ECG signal data
100.hea   → Header information
100.atr   → Beat annotations
```

These files should be read together using the `wfdb` Python package.

## Annotation Symbols

The `.atr` annotation file contains labels for different heartbeat types. Some common MIT-BIH annotation symbols include:

| Symbol | Meaning                               |
| ------ | ------------------------------------- |
| `N`    | Normal beat                           |
| `L`    | Left bundle branch block beat         |
| `R`    | Right bundle branch block beat        |
| `A`    | Atrial premature beat                 |
| `V`    | Premature ventricular contraction     |
| `F`    | Fusion of ventricular and normal beat |
| `/`    | Paced beat                            |
| `Q`    | Unclassifiable beat                   |

For this ECG anomaly detection project, a simple initial labelling approach may be:

```text
Normal class: N
Anomaly class: all other beat types
```

This can later be refined depending on the machine learning approach used.

## Reading the Dataset in Python

Install the required package:

```bash
pip install wfdb matplotlib
```

Example code for reading record `100`:

```python
import wfdb
import matplotlib.pyplot as plt

record_path = "data/raw/mitdb/1.0.0/100"

record = wfdb.rdrecord(record_path)
annotation = wfdb.rdann(record_path, "atr")

print("Signal shape:", record.p_signal.shape)
print("Sampling frequency:", record.fs)
print("Signal names:", record.sig_name)
print("First 10 annotation symbols:", annotation.symbol[:10])
print("First 10 annotation sample positions:", annotation.sample[:10])

# Plot first 10 seconds of the first ECG lead
fs = record.fs
samples = int(10 * fs)

plt.figure(figsize=(12, 4))
plt.plot(record.p_signal[:samples, 0])
plt.title("MIT-BIH Record 100 - First 10 Seconds")
plt.xlabel("Samples")
plt.ylabel("ECG Amplitude")
plt.show()
```

## Notes for Project Use

The dataset will be used for ECG anomaly detection. The initial workflow may include:

1. Reading ECG records using `wfdb`.
2. Extracting ECG signal segments.
3. Reading beat annotations from `.atr` files.
4. Separating normal and abnormal beats.
5. Training an anomaly detection model, such as an autoencoder.
6. Evaluating abnormal ECG detection performance.

## Citation and Acknowledgement

This project uses the MIT-BIH Arrhythmia Database from PhysioNet. Users should cite PhysioNet and the original MIT-BIH Arrhythmia Database when using this dataset in research, reports, or publications.

Dataset link:

https://physionet.org/content/mitdb/1.0.0/

