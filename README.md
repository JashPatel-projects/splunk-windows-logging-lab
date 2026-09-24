\# Enterprise SIEM \& Windows Endpoint Log Ingestion Lab



A hands-on implementation of an automated, real-time log ingestion pipeline streaming Windows Event Logs from a dedicated endpoint VM into a Splunk Enterprise instance hosted on Ubuntu 22.04 LTS, accessed and managed via a host machine browser interface.



\---



📌 Architecture Overview



+------------------------------------+           +------------------------------------+

|         Windows VM (Endpoint)      |           |        Ubuntu 22.04 VM (SIEM)      |

|    (Splunk Universal Forwarder)    |  TCP:9997 |        (Splunk Enterprise)         |

|                                    | --------> |                                    |

|  - Security \& System Logs          | (Bridged) |  - Ingestion Engine (Port 9997)    |

|  - Application Logs                |           |  - Custom Index: "windows"         |

|  - PowerShell Operational Logs     |           |                                    |

+------------------------------------+           +------------------------------------+

&#x20;                                                          ^

&#x20;                                                          | HTTP/8000

&#x20;                                                          | (Bridged Network)

&#x20;                                                +---------------------------------------+

&#x20;                                                |            Host Machine               |

&#x20;                                                |        (Web Browser Client)           |

&#x20;                                                |                                       |

&#x20;                                                |  - Splunk Web UI (192.168.102.75:8000)|

&#x20;                                                |  - SPL Analysis \& Dashboards          |

&#x20;                                                +---------------------------------------+



Indexer \& Web Server: Splunk Enterprise on Ubuntu Linux VM (192.168.102.75)



Forwarder Endpoint: Splunk Universal Forwarder on Windows VM



Management Console: Host Machine running Web Browser (192.168.102.75:8000)



Receiving Port: TCP 9997



Network Adapter Mode: VMware Bridged Mode







📁 Repository Structure



splunk-windows-logging-lab/

├── configs/

│   ├── inputs.conf

│   └── outputs.conf

├── docs/

│   ├── architecture\_and\_config.md

│   ├── troubleshooting\_guide.md

│   └── interview\_and\_presentation\_guide.md

├── screenshots/

│   ├── splunk\_web\_events.png

│   ├── network\_connectivity.png

│   ├── ubuntu\_listening\_port.png

│   └── configuration\_files.png

├── .gitignore

└── README.md







⚙️ Configuration File Specifications



configs/inputs.conf (Windows VM Forwarder)



\[WinEventLog://Security]

disabled = 0

index = windows

start\_from = oldest



\[WinEventLog://System]

disabled = 0

index = windows

start\_from = oldest



\[WinEventLog://Application]

disabled = 0

index = windows

start\_from = oldest



\[WinEventLog://Microsoft-Windows-PowerShell/Operational]

disabled = 0

index = windows

start\_from = oldest





configs/outputs.conf (Windows VM Forwarder)



\[tcpout]

defaultGroup = default-autolb-group



\[tcpout:default-autolb-group]

server = 10.50.182.74:9997







🛠️ Installation \& Setup Workflow



1\. Splunk Enterprise Setup (Ubuntu VM)



\--> Enabled the TCP receiving processor on port 9997:

\--> Created a dedicated destination index named windows via Splunk Web UI on port 8000.

\--> Verified the socket listener status on Ubuntu:





2\. Universal Forwarder Setup (Windows VM)



\-->Installed Splunk Universal Forwarder on the target Windows guest VM.

\--> Configured event log harvesting in C:\\Program Files\\SplunkUniversalForwarder\\etc\\system\\local\\inputs.conf.

\--> Configured forwarding destination pointing to the Ubuntu VM IP (192.168.102.75:9997) in outputs.conf.





3\. Monitoring \& Analysis (Host Machine)



\--> Accessed the Splunk Web Interface from the Host Browser at http://192.168.102.75:8000.

\--> Conducted real-time SPL searches, verified index population, and validated log parsing pipelines.







🛡️ Troubleshooting \& Engineering Highlights



**--> Multi-Node Networking: Standardized all virtual network adapters (Host, Ubuntu VM, Windows VM) to Bridged Mode to eliminate NAT-induced routing isolation and allow full 3-way connectivity.**



**--> Configuration Stanza Alignment: Fixed log forwarding drop issues by ensuring syntax parity between defaultGroup = default-autolb-group and \[tcpout:default-autolb-group] in outputs.conf.**



**--> Socket Listening Audit: Resolved cross-VM connection failures by verifying that splunkd bound correctly to 0.0.0.0:9997 rather than localhost.**







**🎯 Project Presentation \& Interview Summary**



**Project Title: 3-Node Enterprise SIEM \& Windows Log Ingestion Pipeline**



**Pitch: "I engineered a multi-node SIEM lab utilizing an Ubuntu VM running Splunk Enterprise, a Windows VM endpoint with Splunk Universal Forwarder streaming event telemetry over TCP 9997, and my Host machine managing the SIEM interface on port 8000. I handled cross-node bridged networking, created tailored inputs/outputs configs, and debugged layer-4 socket connectivity and configuration syntax errors to achieve real-time log ingestion."**



**Core Competencies: SIEM Architecture, Multi-Node Networking, Splunk Enterprise/UF, SPL, Windows Event Log Telemetry, and Linux System Administration.**

