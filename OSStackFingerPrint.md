# OS Stack Fingerprint

Now collect an independent OS-stack fingerprint for the authorized test device using the known open port and a known closed port.

**Run:**

See the original command in the screenshot below.

Port 1 supplied the closed-port behavior Nmap needed, while 43917 supplied the open-port behavior.

Nmap compares characteristics such as:

- TCP options and ordering.
- Initial window sizes.
- Sequence-number behavior.
- IP ID behavior.
- Reset handling.
- Timestamp behavior.
- TTL characteristics.

Potentially narrow this from an unknown service to Android/Linux, Apple, Windows, or an embedded stack without relying on the application port.

## Output

![Device OS identification](images/device-os-id%281%29.png)

Nmap's stack matcher ranks Android 12 at 91%, with broader Android/Linux families among the other matches, but explicitly says there is no exact match.

## What Nmap Actually Found

| OS Fingerprint | Similarity |
| --- | --- |
| Linux kernel 4.15–5.19 | 92% |
| Android 12 / Linux 5.4 | 91% |
| Android 10–12 | 91% |
| Other Linux, Android, and FreeBSD possibilities | 88–89% |

### Additional Findings

- Explicit warning: "No exact OS matches for host."
- Both the open and closed ports answered with TTL 63.
- Network distance is reported as two hops, which is consistent with Kali → WSL/Windows NAT → Wi-Fi device.

## Most Defensible Conclusion

The device probably uses a Linux-derived TCP/IP stack.

Android is supported by the fingerprint, but not proven. ADB remains a possible explanation for the unidentified service, but the OS fingerprint does not establish its identity.

**Clarification:** Nmap's fingerprint similarity scores are not probabilities of device identity.

**Redaction note:** The original target IP address has been redacted. The original fingerprint results have been retained.

---

[Previous: Target IP](TargetIP.md) | [Return to README](README.md)
