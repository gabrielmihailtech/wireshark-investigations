# Investigation 10 - Antnosience Incident Analysis

## Objective

Investigate suspicious network traffic within the BURNINCANDLE Active Directory environment, identify the compromised workstation and user account, and document the associated indicators of compromise (IOCs).

---

## Executive Summary

Network traffic analysis identified a compromised Windows workstation within the BURNINCANDLE environment.

The affected host repeatedly communicated with the external domain `antnosience.com`, which resolved to IP address `157.245.142.66`. DNS activity, TLS negotiations, and Server Name Indication (SNI) values confirmed the communication between the victim system and the suspicious external infrastructure.

Further analysis of authentication and directory service traffic identified the affected workstation and user account as Patrick Zimmerman operating from workstation DESKTOP-5QS3D5D.

---

## Victim Details

### Host Information

- IP Address: 10.0.19.14
- Hostname: DESKTOP-5QS3D5D
- MAC Address: 2c:54:2d:2f:13:5c

### User Information

- Username: patrick.zimmerman
- Full Name: Patrick Zimmerman

### Domain Information

- Active Directory Domain: burnincandle.com
- Domain Controller: 10.0.19.9

---

## Investigation Process

### Step 1 - Endpoint Analysis

Wireshark Endpoint statistics identified:

```text
10.0.19.14
```

as the most active internal workstation.

The host generated the highest amount of traffic and became the primary focus of the investigation.

---

### Step 2 - Conversation Analysis

The most significant external communication involved:

```text
10.0.19.14
↓
157.245.142.66
```

This conversation generated the largest amount of external traffic observed in the capture.

Additional external communications included:

```text
23.227.198.203
87.238.33.8
```

However, traffic related to `157.245.142.66` was prioritized due to its volume and associated domain activity.

---

### Step 3 - DNS Analysis

DNS traffic revealed repeated queries for:

```text
antnosience.com
```

Responses resolved the domain to:

```text
157.245.142.66
```

Observed sequence:

```text
DNS Query
↓
DNS Response
↓
157.245.142.66
```

---

### Step 4 - TLS Analysis

Traffic to the external IP address was inspected.

TLS Client Hello packets revealed:

```text
SNI = antnosience.com
```

Observed communication:

```text
10.0.19.14
↓
157.245.142.66
↓
TLS / 443
```

The DNS resolution and TLS SNI value both confirmed communication with the same external host.

---

### Step 5 - Host Identification

NBNS traffic revealed workstation registration activity.

Identified hostname:

```text
DESKTOP-5QS3D5D
```

---

### Step 6 - User Identification

Kerberos authentication traffic identified the logged-on user account:

```text
patrick.zimmerman
```

Observed within:

```text
CNameString
```

fields of Kerberos authentication requests.

---

### Step 7 - Full Name Identification

A packet search using:

```text
Zimmerman
```

identified SAMR traffic containing user account information.

SAMR responses revealed:

```text
Full Name: Patrick Zimmerman
```

---

## Indicators of Compromise (IOCs)

### Victim System

```text
IP Address: 10.0.19.14
Hostname: DESKTOP-5QS3D5D
MAC Address: 2c:54:2d:2f:13:5c
Username: patrick.zimmerman
Full Name: Patrick Zimmerman
```

---

### Suspicious Domain

```text
antnosience.com
```

---

### External IP Address

```text
157.245.142.66
```

---

### TLS Evidence

```text
SNI = antnosience.com
```

---

## Findings

- Host 10.0.19.14 was identified as the primary workstation involved in the suspicious activity.
- NBNS traffic identified the hostname DESKTOP-5QS3D5D.
- Kerberos authentication traffic identified the user account patrick.zimmerman.
- SAMR traffic identified the full user name Patrick Zimmerman.
- DNS traffic revealed repeated queries for antnosience.com.
- The domain resolved to 157.245.142.66.
- TLS Client Hello traffic confirmed the destination through the SNI value antnosience.com.
- The largest external conversation observed in the traffic capture involved communications with 157.245.142.66.

---

## Conclusion

The investigation identified workstation 10.0.19.14 (DESKTOP-5QS3D5D) as the system involved in the suspicious activity.

The affected user account was identified as Patrick Zimmerman (patrick.zimmerman). Network traffic showed repeated communication with the external domain `antnosience.com`, which resolved to IP address `157.245.142.66` and was confirmed through TLS SNI analysis.

The identified domain and IP address should be treated as indicators of compromise and included in monitoring, detection, and response activities.
