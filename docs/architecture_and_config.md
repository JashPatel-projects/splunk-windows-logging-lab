# Lab Architecture & Configuration Specification

## Network Specifications
- **Host Subnet**: Local Bridged Subnet
- **Splunk Enterprise (Ubuntu 22.04)**: `192.168.102.75`
- **Splunk Web GUI**: Port `8000`
- **Splunk Receiver Port**: Port `9997`
- **Windows Forwarder Host**: Local Windows Machine (Bridged Adapter)

## Ingestion Settings
- **Destination Index**: `windows`
- **Target Event Logs**: Security, System, Application, PowerShell Operational
- **Data Ingestion Mode**: Historical backfill (`start_from = oldest`) + Real-time streaming