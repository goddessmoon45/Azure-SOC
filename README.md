# Building a SOC + Honeynet in Azure (Live Traffic)
![Honeynet + (1)](https://github.com/goddessmoon45/Azure-SOC/assets/144558304/48db73cd-7c47-4aac-a0de-915a34eafbbf)



## Introduction

This project was really fun to create as I was able to build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured  security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups (NSG) and built-in firewalls completely open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups(NSG) were hardened by blocking ALL traffic with the exception of my own admin workstation(my IP address), and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
WINDOWS RDP AUTH
![windows rdp auth](https://github.com/goddessmoon45/Azure-SOC/assets/144558304/d6ba9771-b783-4971-823d-74da91e2636a)

MALICIOUS NSG ALLOWED IN
![malicious nsg allowed in](https://github.com/goddessmoon45/Azure-SOC/assets/144558304/aae8aaa5-59b3-4b26-a921-50fa406aaf27)

LINUX SSH FAIL
![linux ssh fail](https://github.com/goddessmoon45/Azure-SOC/assets/144558304/d872d273-bc1c-4618-ae5e-fa7cd0491d72)

MSSQL AUTH FAIL
![mssql auth fail](https://github.com/goddessmoon45/Azure-SOC/assets/144558304/c3d56045-2338-4317-afff-712d2ba352e2)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time	2024-06-10T14:32:25.1699288Z
Stop Time	  2024-06-11T14:32:25.1699288Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 14358
| Syslog                   | 2345
| SecurityAlert            | 6
| SecurityIncident         | 73
| AzureNetworkAnalytics_CL | 103
## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
	
Start Time	2024-06-12T13:33:26.8036683Z
Stop Time   2024-06-13T13:33:26.8036683Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 400
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
