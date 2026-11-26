# Network Discovery with beginner guide 

A network discovery and device fingerprinting lab using Windows, WSL2, Kali Linux and evidence-based analysis.

## About the Project

This project originated from a practical network investigation in which I examined device identity, network traffic, service behavior and Windows cross-device communication.

Rather than relying on a single tool or fingerprint, the investigation compares multiple sources of evidence to establish what can be confirmed, what remains a hypothesis and what cannot be determined.

The Beginner Edition was developed from my original investigation to allow others to explore the same analytical process within their own authorized test environments.

## Investigation Methodology

The original investigation follows a consistent structure:

- **Run:** The command or test performed.
- **Output:** The observed response.
- **Findings:** Interpretation of the evidence and its limitations.

Device identification and service identification are treated as separate conclusions.

## Tools and Technologies

| Tool | Purpose |
|---|---|
| Windows PowerShell | Host-side network analysis |
| WSL2 | Linux environment within Windows |
| Kali Linux | Network analysis environment |
| Nmap | Network and service discovery |
| Wireshark | Packet capture and analysis |
| TShark | Command-line packet analysis |
| Npcap | Windows packet-capture driver |
| arp-scan | Local network discovery |
| OpenSSL | TLS analysis |
| Python 3 | Supplemental protocol analysis |

## Project Status

**Version 0.1 — Beginner Beta**

The original investigation documents historical observations. The adapted beginner workflow is awaiting independent reproduction and testing.

Identifying information has been redacted or replaced with documentation values.

## Authorized Use

This material is intended for educational research and authorized network analysis. Only test devices and networks you own or have explicit permission to examine.

## Copyright

Copyright © 2026. All rights reserved.

No license to reproduce, redistribute, modify or commercially use the original material is granted except where permitted by law or expressly authorized by the copyright holder.
