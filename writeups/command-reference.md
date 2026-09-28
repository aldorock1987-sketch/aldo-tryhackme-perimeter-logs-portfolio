# Command Reference

## Navigate to the lab logs

```bash
cd ~/Desktop/Perimeter_logs/challenge
pwd
ls
```

Expected files:

```text
firewall.log
ids_alerts.log
vpn_auth.log
```

## Reconnaissance

```bash
cat firewall.log | grep "BLOCK" | head
cat firewall.log | grep "BLOCK" | cut -d' ' -f5 | cut -d: -f1 | sort -nr | uniq -c
```

## VPN failures

```bash
cat vpn_auth.log | grep FAIL | cut -d' ' -f3 | sort -nr | uniq -c
cat vpn_auth.log | grep svc_backup
```

## Lateral movement / SMB

```bash
cat firewall.log | grep "10.8.0.23" | grep "ALLOW"
cat ids_alerts.log | grep "10.8.0.23" | grep "SMB"
```

## C2

```bash
cat ids_alerts.log | grep C2 | head
```

## Exfiltration

```bash
grep -i "Exfiltration" ids_alerts.log
```

## Useful SOC habits

```text
1. Identify an indicator.
2. Pivot on that indicator.
3. Correlate with another log source.
4. Validate the activity.
5. Build the attack timeline.
```
