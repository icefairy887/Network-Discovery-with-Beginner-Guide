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

## Project Documentation

### Beginner Resources

- [Beginner Setup Guide](SETUP.md)


### Original Investigation

1. [Device Discovery](DeviceDiscovery.md)
2. [Full TCP Discovery](FullTCPDiscovery.md)
3. [The Unknown Device and TCP Service](TargetIP.md)
4. [OS Stack Fingerprint](OSStackFingerPrint.md)
5. [IPv6 Fingerprint](IPV6FingerPrint.md)
6. [ADB Verification](ADBverification.md)
7. [WSL NAT Bypass](WSLNATBypass.md)
8. [Windows PID — Owner of UDP Port](winPIDownerofUDPport.md)
9. [Link Target IP to Device Record](linktargetIPtodevicerecord.md)
10. [Evidence Chain](EvidenceChain.md)
11. [Conclusion](Conclusion.md)

### OneNote Notebook

- [Network Discovery — Beginner Edition](https://1drv.ms/o/c/4698bd72d12c7cec/IgC4YIdFFolrQZaWuW0jhaLHAdtUC5kvYJxnrEmiZnyrEt8?e=2hBrrY)

## Project Status

**Version 0.1 — Beginner Beta**

The original investigation documents historical observations. The adapted beginner workflow is awaiting independent reproduction and testing.

Identifying information has been redacted or replaced with documentation values.

## Authorized Use

Only test devices and networks you own or have explicit permission to examine.

## References

[Microsoft's WSL networking documentation](https://learn.microsoft.com/windows/wsl/networking) 

[NAT is WSL's default topology](https://learn.microsoft.com/windows/wsl/networking)

[AOSP ADB transport protocol](https://android.googlesource.com/platform/packages/modules/adb/+/refs/heads/android14-release/protocol.txt)

## Copyright

Copyright © 2026. All rights reserved.

No license to reproduce, redistribute, modify or commercially use the original material is granted except where permitted by law or expressly authorized by the copyright holder.
