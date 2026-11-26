# Evidence Chain

1. The packet capture showed:
   - Windows PC `10.00.00.4:55000` ⇄ `10.00.00.3:36000`

2. Windows showed that UDP port `55000` belonged to:
   - CrossDeviceService.exe

3. The CrossDeviceService metadata showed that the linked Android device known to that service was:
   - Display name: [PHONE]
   - Manufacturer: Samsung
   - Model: [REDACTED]
   - ClientType: LTW

4. Independent Nmap fingerprinting of `10.00.00.3` favored Linux/Android-family systems, although it had no exact OS match.

5. The original MAC for `10.00.00.3`, represented here as `02:11:22:33:44:55`, has the locally administered bit set. That is consistent with a phone using randomized Wi-Fi MAC addressing, but it does not identify Samsung or that particular phone.

6. The S25's Wi-Fi settings directly reported:
   - Randomized MAC: `02:11:22:33:44:55`
   - IPv4 address: `10.00.00.3`

7. Those exactly matched the network evidence:
   - Windows neighbor table: `10.00.00.3` → `02-11-22-33-44-55`
   - Packet capture source: `10.00.00.3`

8. `10.00.00.3` was identified as the S25 Edge used in the investigation.

9. **S25 identification confirmed. Port 43917 identification still unresolved.**

---

**Redaction note:** IP addresses, MAC addresses, UDP ports and personal device identifiers have been replaced with consistent synthetic values. The original evidence relationships and investigative conclusions have been preserved.

---

[Previous: Link Target IP to Device Record](linktargetIPtodevicerecord.md) | [Return to README](README.md)
