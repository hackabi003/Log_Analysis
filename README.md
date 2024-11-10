# Windows System Logs Anomaly Detection

## Overview
This repository provides a Python script to read Windows Event Viewer logs and detect anomalies using the Isolation Forest algorithm. The tool is designed to help cybersecurity professionals identify suspicious activities in system logs by leveraging machine learning techniques.

## Features
- **Log Retrieval**: Reads logs from the Windows Event Viewer, including categories such as 'System', 'Application', and 'Security'.
- **Preprocessing**: Encodes relevant fields for machine learning.
- **Anomaly Detection**: Uses the Isolation Forest model to flag potential anomalies.
- **Output**: Saves detected anomalies to a CSV file for further analysis.

## Requirements
- Python 3.x
- Libraries: 
  - `pandas`
  - `scikit-learn`
  - `pywin32`

## Installation
1. **Clone the repository**:
   git clone https://github.com/hackabi003/windows-log-anomaly-detection.git
   cd windows-log-anomaly-detection

2. **Install required packages**:
   pip install pandas scikit-learn pywin32

## Usage
1. **Run the script**:
   python detect_anomalies.py
   Ensure that you have the necessary permissions to read Event Viewer logs (run as an administrator if needed).

2. **Output**:
   - The script reads logs from the Event Viewer and processes them.
   - Detected anomalies are saved to `anomalies.csv` in the current directory.

## Script Details
### `read_system_logs()`
- Reads logs from the specified Event Viewer type (default is 'System').
- Extracts key fields such as `EventID`, `Category`, `TimeGenerated`, `SourceName`, `EventType`, and `Message`.

### `detect_anomalies(logs_df)`
- Encodes categorical features using `LabelEncoder`.
- Applies `IsolationForest` to identify anomalies.
- Returns a DataFrame with anomalies flagged (`-1` for anomaly, `1` for normal).

## Customization
- **Log Type**: Modify `log_type` in the `read_system_logs()` function to analyze different logs (e.g., 'Security').
- **Model Tuning**: Adjust the `contamination` parameter in the `IsolationForest` model to change the sensitivity of anomaly detection.

## Example Output
```
Detected 10 anomalies.
   EventID  Category        TimeGenerated     SourceName  EventType  Anomaly
0      4673      1345  2024-11-10 10:15:03  Security      1         -1
...
```

## License
This project is licensed under the MIT License.

## Contributing
Feel free to open issues or submit pull requests if you have suggestions or improvements.

## Acknowledgements
- This project uses the `pywin32` library for accessing Windows Event Viewer logs.
- The `IsolationForest` algorithm from `scikit-learn` provides robust anomaly detection capabilities.

---

**Disclaimer**: Use this tool responsibly and ensure you have authorization when analyzing system logs on any machine.

