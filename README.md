# Mini SOC – Splunk Lab

A hands-on Security Operations Center (SOC) lab built with Splunk Enterprise, Docker, and Windows 11.

This project demonstrates centralized Windows event log collection, security monitoring, log analysis, and basic detection engineering.

## 1. Lab Architecture

| Component | Technology |
|---|---|
| SIEM | Splunk Enterprise 10.4.5 |
| Deployment | Docker Desktop |
| Host OS | macOS – Apple Silicon |
| Endpoint | Windows 11 ARM64 |
| Virtualization | UTM |
| Log Forwarder | Splunk Universal Forwarder 10.4.3 |
| Endpoint Monitoring | Microsoft Sysmon 15.22 (installed; ingestion not configured) |

## 2. Implemented Features

- Deployed Splunk Enterprise using Docker.
- Configured Windows Universal Forwarder.
- Forwarded Windows event logs to Splunk.
- Collected Security, System, and Application logs.
- Verified successful event ingestion.
- Investigated failed Windows logon attempts.
- Developed and tested a SPL detection query for Event ID 4625.

## 3. Detection Rules

### Failed Windows Logons – Event ID 4625

**Objective:** Identify and summarize failed authentication attempts.

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4625
| fillnull value="Unknown" Account_Name Workstation_Name Source_Network_Address
| stats count by host Account_Name Workstation_Name Source_Network_Address
| sort -count
```

**Investigation fields:**
- Host
- Account name
- Workstation name
- Source network address
- Number of failed attempts

The query was tested against Windows Security events collected in the lab.

## 4. Repository Structure

```text
mini-soc-splunk-lab/
├── detection-rules/
│   └── failed-logons.spl
├── windows-forwarder/
│   ├── inputs.conf
│   └── outputs.conf
├── docs/
│   └── architecture.md
├── README.md
└── .gitignore
```

## 5. Windows Forwarder Configuration

The `windows-forwarder/` directory contains example configuration files.

- `inputs.conf`: Defines Windows event log sources.
- `outputs.conf`: Defines the destination Splunk server.

Replace `SPLUNK_SERVER_IP` in the example configuration with your own Splunk server address before deployment.

## 6. Security Considerations

- No passwords or authentication tokens should be committed.
- No private keys or sensitive configuration files should be published.
- Local IP addresses should be replaced with placeholders.
- Raw event logs and backups should remain private.
- Screenshots must be reviewed for sensitive information before publication.

## 7. Future Improvements

- Additional Windows Event ID detection rules.
- Sysmon-based threat detection.
- Authentication monitoring dashboards.
- Security alerting and investigation workflows.
- SOAR integration.

## 8. Disclaimer

This project is intended for educational purposes and defensive cybersecurity research in a controlled lab environment.
