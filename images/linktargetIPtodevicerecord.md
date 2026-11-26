# Link Target IP to Device Record

For decisive proof, compare the phone's current Wi-Fi MAC address with the previously observed MAC address:

`02:11:22:33:44:55`

On the Samsung, open the connected Wi-Fi network's details and look for Phone MAC address or Randomized MAC address.

An exact MAC match supports the association between that Wi-Fi identity and the S25. If it doesn't match, the target cannot be attributed to the phone under that current Wi-Fi identity, regardless of how persuasive the other clues looked.

## Findings From S25 Wi-Fi Settings

![Android mobile Wi-Fi settings](images/android-mobile-wifi-settings.png)

The phone's current network identifiers matched the previously observed network evidence.

| Identifier | Sanitized Value |
| --- | --- |
| IPv4 | 10.00.00.3 |
| Randomized MAC | 02:11:22:33:44:55 |

---

## Controlled Disappearance/Reappearance Test

Turn wifi off on S25 and then run this in Kali: sudo nmap -n -Pn -p43917 --reason --max-retries 1 --host-timeout 15s 10.0.0.3


## Output

![Controlled Wi-Fi on-off test](images/on-off-test.png)

```text
43917/tcp filtered unknown no-response
```

Earlier, port 43917 answered with a SYN-ACK and was reported open. Now it gave no response.

That means only:

Whatever controls TCP port 43917 stopped answering unsolicited connection attempts.

It does not necessarily mean the device went offline.

Because of `-Pn`, Nmap printed:

```text
Host is up, received user-set.
```

That line is not an independent observation. `-Pn` means assume the host is up and scan it anyway.

Possible explanations for the port changing from open to filtered include:

- The application stopped listening.
- The service entered a sleep or locked state.
- A host firewall began silently dropping probes.
- The service only opens while a cross-device session is active.
- The device rate-limited repeated scanning.
- The device kept its IP but changed operational state.

These are possible explanations, not confirmed causes.

---

## Windows CrossDevice Communication

Check whether the Windows CrossDevice UDP conversation disappears.

**Run in Kali:**

```bash
"/mnt/c/Users/<USERNAME>/Downloads/WiresharkPortable64/App/Wireshark/tshark.exe" -i 5 -n -a duration:20 -f "host 10.00.00.3"
```

**Redaction note:** The Windows username and target IP address have been replaced with placeholders.

## Output Findings

The physical Wi-Fi capture directly observed packets originating from:

```text
10.00.00.3 → 224.0.0.251
```

224.0.0.251 is the IPv4 multicast address for mDNS.

The target was actively searching for Google Cast services:

```text
_googlecast._tcp.local
```

It also searched several Google Cast subtype and service-instance identifiers:

```text
[REDACTED]._sub._googlecast._tcp.local
[REDACTED]._sub._googlecast._tcp.local
[REDACTED]._googlecast._tcp.local
```

Those identifiers are useful fingerprints.

They suggest the target participates in the Google Cast ecosystem, which is consistent with an Android phone, tablet, Google TV controller, Chrome-based device, or an application using the Cast SDK.

They still do not independently establish that the device is a Samsung S25 Edge.

## Conclusion

- The phone's current Wi-Fi identifiers matched the network evidence.
- TCP port 43917 changed from open to filtered.
- Google Cast mDNS queries were observed.
- The identity of the application behind TCP port 43917 remains unresolved.

**Clarification:** Google Cast queries indicate service discovery activity, not necessarily that the device hosts a Cast receiver.

**Redaction note:** Original IP addresses, MAC addresses, personal paths and identifying service-instance values have been redacted or replaced with synthetic values.

---

[Previous: Windows PID — Owner of UDP Port](winPIDownerofUDPport.md) | [Return to README](README.md)
