# Mini SOC Architecture

## Components

- macOS on Apple Silicon: Docker Desktop host.
- Splunk Enterprise 10.4.5: runs in a Docker container.
- Windows 11 ARM64: endpoint running in UTM.
- Splunk Universal Forwarder 10.4.3: forwards Windows Event Logs to Splunk over TCP port 9997.
- Sysmon 15.22: installed on the Windows endpoint; Sysmon event ingestion into Splunk is not yet configured.

## Event Flow

1. Windows generates Security, System, and Application events.
2. Splunk Universal Forwarder collects the configured Windows Event Logs.
3. The forwarder sends events to the Splunk receiving port (TCP 9997).
4. Splunk indexes the events in the main index.
5. Analysts search and investigate events using SPL.

## Current Detection

Windows Security Event ID 4625 (failed logon) is queried and grouped by host, account, workstation, and source address.

## Network Configuration

The forwarder output configuration uses a placeholder server address. Replace SPLUNK_SERVER_IP with the reachable Splunk host IP in your local environment. Do not commit private IP addresses, credentials, or tokens.
