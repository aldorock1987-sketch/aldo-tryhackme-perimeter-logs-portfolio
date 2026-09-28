# Investigation Notes

## 1. Reconnaissance

Start with blocked firewall traffic because blocked connections are useful for identifying repeated probing against internal services.

```bash
cat firewall.log | grep "BLOCK" | head
```

To count the source addresses associated with blocked requests, the lab uses:

```bash
cat firewall.log | grep "BLOCK" | cut -d' ' -f5 | cut -d: -f1 | sort -nr | uniq -c
```

The highest-volume source is `203.0.113.45` with 279 blocked requests.

The investigation then pivots on that IP to look for allowed traffic and related activity in other logs.

## 2. VPN credential access

Search for failed VPN authentication attempts:

```bash
cat vpn_auth.log | grep FAIL | cut -d' ' -f3 | sort -nr | uniq -c
```

The suspicious source produces a large number of failures. Narrowing the search to the suspicious source reveals repeated failures against `svc_backup`, followed by successful authentication.

The first successful session assigns `10.8.0.23`.

## 3. Lateral movement

Once an internal IP is identified, pivot into the firewall log and inspect allowed connections from the compromised host.

The lab shows connections to ports `22`, `445`, and `3389` on internal systems. The IDS records alerts labelled `Possible MS-SMB Lateral Movement` for traffic to port `445`.

This establishes the SMB-related lateral-movement indicator used in the exercise.

## 4. C2 beaconing

Search IDS alerts for C2 indicators:

```bash
cat ids_alerts.log | grep C2 | head
```

The relevant alerts are labelled `ET TROJAN Possible C2 Beaconing` and repeat over time, consistent with beaconing behaviour.

The exercise identifies `10.0.0.60` as the internal host that beaconed to the C2 and `198.51.100.77` as the IP associated with the C2 activity.

## 5. Data exfiltration attempts

Search the IDS data for the exfiltration classification:

```bash
grep -i "Exfiltration" ids_alerts.log
```

The relevant alerts are labelled `Potential Data Exfiltration` and describe large HTTP POST uploads to external destinations.

The host identified by the exercise for these exfiltration attempts is `10.0.0.51`.

## 6. Final incident chain

```text
203.0.113.45
  |
  +-- reconnaissance / probing
  |
  +-- VPN credential attacks against svc_backup
          |
          +-- successful VPN access
                  |
                  +-- 10.8.0.23
                          |
                          +-- lateral movement
                                  |
                                  +-- SMB / TCP 445
                                          |
                                          +-- 10.0.0.60 beaconing to C2
                                                  |
                                                  +-- 198.51.100.77
                                                          |
                                                          +-- exfiltration indicators
                                                                  |
                                                                  +-- 10.0.0.51
```

> All indicators in this document refer to the synthetic lab environment.
