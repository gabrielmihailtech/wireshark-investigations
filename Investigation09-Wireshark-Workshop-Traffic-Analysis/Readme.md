# Investigation 09 - Wireshark Workshop Traffic Analysis

## Objective

Analyze network traffic from the WIRESHARKWORKSHOP environment and identify the primary workstation, associated user account, and notable external communications.

---

## Executive Summary

Network traffic analysis identified a Windows workstation generating the majority of network activity within the captured environment.

The investigation identified the workstation, associated user account, hostname, and Active Directory environment. External communications primarily involved legitimate software-development and content-distribution infrastructure, including GitHub and Maven repository services.

No clear malware payload delivery, command-and-control activity, or malicious infrastructure was identified during the reviewed traffic.

---

## Victim Details

### Host Information

- IP Address: 172.16.1.66
- Hostname: DESKTOP-SKBR25F
- MAC Address: 00:15:fa:50:6f:18

### User Information

- Username: ccollier
- Full Name: Clark Collier

### Active Directory Environment

- Domain: WIRESHARKWORKSHOP.ONLINE
- Domain Controller: 172.16.1.4

---

## Investigation Process

### Step 1 - Endpoint Analysis

Wireshark Endpoint statistics identified:

```text
172.16.1.66
```

as the most active internal host.

This host generated the highest amount of traffic and became the primary focus of the investigation.

---

### Step 2 - Conversation Analysis

Major external communications involved:

```text
199.232.196.209
185.199.110.133
```

and several Microsoft/Akamai-related systems.

---

### Step 3 - DNS Analysis

DNS activity revealed successful resolution of:

```text
repo1.maven.org
```

which resolved through:

```text
dualstack.sonatype.map.fastly.net
```

to:

```text
199.232.196.209
```

Additional DNS activity revealed:

```text
objects.githubusercontent.com
```

which resolved to:

```text
185.199.110.133
```

---

### Step 4 - TLS Analysis

TLS Client Hello traffic confirmed the following Server Name Indications (SNI):

```text
repo1.maven.org
```

and

```text
objects.githubusercontent.com
```

Observed communications matched the corresponding DNS resolutions.

---

### Step 5 - Host Identification

NetBIOS and authentication traffic identified:

```text
DESKTOP-SKBR25F
```

as the workstation hostname.

---

### Step 6 - User Identification

Kerberos authentication traffic revealed:

```text
ccollier
```

as the logged-in user.

Further investigation of SAMR traffic identified:

```text
Clark Collier
```

as the full user name.

---

## Indicators of Interest

### Victim System

```text
IP Address: 172.16.1.66
Hostname: DESKTOP-SKBR25F
MAC Address: 00:15:fa:50:6f:18
Username: ccollier
Full Name: Clark Collier
```

### External Infrastructure Observed

```text
199.232.196.209
repo1.maven.org
```

```text
185.199.110.133
objects.githubusercontent.com
```

---

## Findings

- Host 172.16.1.66 generated the largest amount of observed traffic.
- Hostname DESKTOP-SKBR25F was identified through authentication and workstation-related traffic.
- Kerberos traffic identified the username ccollier.
- SAMR traffic revealed the full name Clark Collier.
- DNS and TLS analysis identified communications with repo1.maven.org and objects.githubusercontent.com.
- Communications observed during the investigation appeared consistent with legitimate software development or software distribution infrastructure.

---

## Conclusion

The investigation identified workstation 172.16.1.66 (DESKTOP-SKBR25F) as the primary active host in the capture.

Analysis identified Clark Collier (ccollier) as the associated user account and confirmed communications with GitHub and Maven-related infrastructure.

Based on the reviewed traffic, no clear evidence of malware delivery, malicious payload retrieval, or command-and-control activity was identified during the investigation.
