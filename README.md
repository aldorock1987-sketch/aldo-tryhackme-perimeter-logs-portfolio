[README.md](https://github.com/user-attachments/files/32775001/README.md)
# aldo-tryhackme-perimeter-logs-portfolio
TryHackMe SOC investigation focused on perimeter log analysis, VPN brute-force detection, lateral movement, C2 beaconing, and data exfiltration using Linux CLI and security logs.
[README_ES.md](https://github.com/user-attachments/files/32775003/README_ES.md)# Investigación SOC: Reconstrucción de un ataque mediante logs perimetrales

Caso práctico basado en un laboratorio de TryHackMe con análisis de logs de firewall, IDS y VPN.

> **Entorno:** red ficticia de Initech Corp / logs sintéticos  
> **Objetivo:** reconstruir la actividad del atacante y correlacionar los indicadores entre distintas fuentes.

## Fuentes analizadas

- `firewall.log`
- `ids_alerts.log`
- `vpn_auth.log`

El análisis se realizó desde Linux utilizando `grep`, `cut`, `sort` y `uniq`. El laboratorio también incluye Splunk para realizar el análisis de forma centralizada.

## Cadena del incidente

```text
Reconocimiento externo
        ↓
Ataques contra credenciales VPN
        ↓
Login VPN exitoso
        ↓
Movimiento lateral
        ↓
Comunicación C2
        ↓
Intentos de exfiltración
```

## Hallazgos principales

| Fase | Indicador |
|---|---|
| Reconocimiento | `203.0.113.45` |
| Host interno escaneado | `10.0.0.20` |
| Usuario atacado en VPN | `svc_backup` |
| IP asignada por VPN | `10.8.0.23` |
| SMB lateral | `445/TCP` |
| Host que beaconiza al C2 | `10.0.0.60` |
| IP asociada al C2 | `198.51.100.77` |
| Host con intentos de exfiltración | `10.0.0.51` |

## Qué demuestra este proyecto

La parte importante del ejercicio es el **pivoting** entre fuentes de datos. Un indicador encontrado en un log se utiliza para buscar la siguiente fase del incidente en otro log.

```text
IP externa
   ↓
reconocimiento
   ↓
VPN
   ↓
usuario comprometido
   ↓
IP interna
   ↓
movimiento lateral
   ↓
C2
   ↓
exfiltración
```

## Habilidades

- Análisis de logs desde Linux
- Correlación de IOCs
- Investigación de firewall
- Análisis de autenticación VPN
- Análisis de alertas IDS
- Reconocimiento y brute force
- Movimiento lateral
- C2 beaconing
- Indicadores de exfiltración
- Reconstrucción de línea temporal

## Respuestas finales del laboratorio

1. `203.0.113.45`
2. `10.0.0.20`
3. `svc_backup`
4. `10.8.0.23`
5. `445`
6. `10.0.0.60`
7. `198.51.100.77`
8. `10.0.0.51`
