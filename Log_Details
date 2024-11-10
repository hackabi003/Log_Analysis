import win32evtlog
import pandas as pd
from sklearn.ensemble import IsolationForest
from sklearn.preprocessing import LabelEncoder

# Function to read logs from Windows Event Viewer
def read_system_logs():
    log_type = 'System'  # You can also use 'Application', 'Security', etc.
    server = 'localhost'  # Local machine
    log_handle = win32evtlog.OpenEventLog(server, log_type)
    
    logs = []
    flags = win32evtlog.EVENTLOG_BACKWARDS_READ | win32evtlog.EVENTLOG_SEQUENTIAL_READ
    total_events = win32evtlog.GetNumberOfEventLogRecords(log_handle)
    
    while True:
        events = win32evtlog.ReadEventLog(log_handle, flags, 0)
        if not events:
            break
        for event in events:
            # Extracting some fields from the logs (you can extract more)
            log_entry = {
                'EventID': event.EventID,
                'Category': event.EventCategory,
                'TimeGenerated': event.TimeGenerated,
                'SourceName': event.SourceName,
                'EventType': event.EventType,
                'Message': event.StringInserts  # Message could be a list
            }
            logs.append(log_entry)
    
    # Close the log handle
    win32evtlog.CloseEventLog(log_handle)
    
    return pd.DataFrame(logs)

# Function to perform anomaly detection on system logs
def detect_anomalies(logs_df):
    # Displaying the first few rows to understand the structure
    print(logs_df.head())
    
    # Encoding relevant categorical features like EventID or other useful fields
    le = LabelEncoder()
    logs_df['EventID'] = le.fit_transform(logs_df['EventID'])
    
    # Applying Isolation Forest for anomaly detection
    model = IsolationForest(contamination=0.01)  # 1% of data as anomalies
    logs_df['Anomaly'] = model.fit_predict(logs_df[['EventID']])
    
    # -1 indicates anomaly, 1 indicates normal
    anomalies = logs_df[logs_df['Anomaly'] == -1]
    return anomalies

# Example usage
logs_df = read_system_logs()
anomalies_df = detect_anomalies(logs_df)

# Save anomalies to a CSV file
anomalies_df.to_csv('anomalies.csv', index=False)
print(f"Detected {len(anomalies_df)} anomalies.")
print(anomalies_df.head(50))
