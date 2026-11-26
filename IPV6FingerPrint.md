# IPv6 Fingerprint

The IPv6 data gives another useful fingerprint.

The phone also shows:

- `fe80::11:22ff:fe33:4455`
- `2001:db8:1:0:11:22ff:fe33:4455`
- `2001:db8:1:0:1234:5678:9abc:def0`

The first two embed an EUI-64-style interface identifier derived from the randomized MAC:

**MAC Address:**

02:11:22:33:44:55

         ↓

00:11:22:ff:fe:33:44:55

The first byte changes from `02` to `00` because IPv6 flips the universal/local bit when constructing the modified EUI-64 identifier.

The final IPv6 address does not visibly embed the MAC and was identified in the original investigation as a possible privacy/temporary address.

## IPv4 and IPv6 Correlation

Earlier IPv4-only work was incomplete: the phone is actively IPv6-capable, and its IPv6 connections are not included in an IPv4-only capture filter.

There are now multiple correlated network identifiers for the same phone:

| Identifier | Sanitized Value |
| --- | --- |
| IPv4 | `10.00.00.3` |
| IPv6 link-local | `fe80::11:22ff:fe33:4455` |
| IPv6 stable | `2001:db8:1:0:11:22ff:fe33:4455` |
| IPv6 temporary candidate | `2001:db8:1:0:1234:5678:9abc:def0` |
| MAC | `02:11:22:33:44:55` |

## Findings

The temporary IPv6 address may rotate, but the MAC-derived IPv6 address is likely to remain correlated with this randomized Wi-Fi identity until the phone or network changes its addressing configuration.

**Clarification:** An IPv6 address that does not visibly embed a MAC is not automatically a temporary address.

**Redaction note:** Original IPv4, IPv6 and MAC addresses have been replaced with illustrative values. The modified EUI-64 relationship has been preserved.

---

[Previous: OS Stack Fingerprint](OSStackFingerPrint.md) | [Return to README](README.md)
