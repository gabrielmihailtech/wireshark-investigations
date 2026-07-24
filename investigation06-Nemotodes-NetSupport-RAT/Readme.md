# Investigation 06 - Nemotodes NetSupport RAT Infection

## Objective

Investigate suspicious network traffic within the NEMOTODES environment, identify the compromised workstation, associated user account, infection vector, and indicators of compromise related to NetSupport RAT activity.

---

## Executive Summary

Network traffic analysis identified a compromised Windows workstation that accessed a suspicious website and subsequently communicated with infrastructure associated with NetSupport RAT.

The infected host accessed the domain `moandcrackedapk.com`, which resolved to IP address `193.42.38.139`. Shortly afterward, the host established repeated communications with `194.180.191.64` using HTTP traffic over TCP port 443.

The traffic contained the User-Agent string `NetSupport Manager/1.3` and repeated POST requests to `/fakeurl.htm`, matching known NetSupport RAT communication patterns.

The affected user was identified as Oliver Q. Boomwald.

---

## Victim Details

### Host Information

- IP Address: 10.11.26.183
- Hostname: DESKTOP-B8TQK49
- MAC Address: d0:57:7b:ce:fc:8b

### User Information

- Username: oboomwald
- Full Name: Oliver Q. Boomwald
- Active Directory Domain: NEMOTODES.HEALTH

---

## Investigation Process

### Step 1 - Endpoint Analysis

Wireshark Endpoint statistics identified:

```text
10.11.26.183
```

as the most active internal host.

The system was responsible for the largest volume of traffic and became the primary focus of the investigation.

---

### Step 2 - DNS Analysis

DNS traffic revealed communications with the following suspicious domain:

```text
moandcrackedapk.com
```

The domain resolved to:

```text
193.42.38.139
```

TLS traffic later confirmed communications with the same domain through SNI information.

---

### Step 3 - TLS Analysis

A TLS Client Hello packet revealed:

```text
SNI = moandcrackedapk.com
```

Observed communication:

```text
10.11.26.183
↓
193.42.38.139
↓
443/TLS
```

This domain represents the likely initial access or malware delivery infrastructure.

---

### Step 4 - Host Identification

NBNS traffic identified the workstation:

```text
DESKTOP-B8TQK49
```

---

### Step 5 - User Identification

Kerberos traffic revealed:

```text
CNameString = oboomwald
```

Further investigation of SAMR traffic identified:

```text
Oliver Q. Boomwald
```

as the compromised user account.

---

### Step 6 - Command and Control Activity

HTTP communications revealed repeated requests from the victim to:

```text
194.180.191.64
```

Observed URI:

```text
http://194.180.191.64/fakeurl.htm
```

Observed User-Agent:

```text
NetSupport Manager/1.3
```

The victim host repeatedly transmitted POST requests and received HTTP 200 OK responses.

The traffic pattern strongly indicates active NetSupport RAT command-and-control communications.

---

## Indicators of Compromise (IOCs)

### Victim System

- 10.11.26.183
- DESKTOP-B8TQK49
- d0:57:7b:ce:fc:8b
- oboomwald
- Oliver Q. Boomwald

---

### Domains

```text
moandcrackedapk.com
```

---

### External IP Addresses

```text
193.42.38.139
194.180.191.64
```

---

### URLs

```text
http://194.180.191.64/fakeurl.htm
```

---

### User-Agent

```text
NetSupport Manager/1.3
```

---

## Findings

- Host 10.11.26.183 was identified as the compromised workstation.
- NBNS traffic identified the hostname DESKTOP-B8TQK49.
- Kerberos traffic identified the user account oboomwald.
- SAMR traffic identified the full name Oliver Q. Boomwald.
- DNS and TLS traffic confirmed communications with moandcrackedapk.com.
- The domain resolved to 193.42.38.139.
- Repeated HTTP POST requests were sent to 194.180.191.64.
- Traffic used the User-Agent string NetSupport Manager/1.3.
- Communication patterns were consistent with NetSupport RAT activity.

---

## Conclusion

The investigation determined that workstation 10.11.26.183 (DESKTOP-B8TQK49) was compromised and communicated with infrastructure associated with NetSupport RAT.

The infected host accessed moandcrackedapk.com and subsequently established command-and-control communications with 194.180.191.64 using NetSupport Manager traffic.

The compromised user account was identified as Oliver Q. Boomwald (oboomwald). The identified domains, IP addresses, URLs, 
and User-Agent strings should be considered indicators of compromise and used for detection and containment activities.
