# Investigation 03 - Lumma In The Room

## Objective

Investigate network traffic associated with a Lumma Stealer alert and identify the affected host, user account, suspicious domains, and indicators of compromise.

---

## Investigation Summary

A traffic capture generated a Lumma Stealer detection associated with communications to a suspicious external IP address.

The investigation focused on identifying the affected workstation, the associated user account, and the suspicious domains contacted during the activity.

---

## Investigation Process

### Step 1 - Endpoint Analysis

Wireshark Endpoint statistics were reviewed to identify the most active internal hosts.

The most active internal system was:

- IP Address: 10.1.21.58

This host became the primary focus of the investigation.

---

### Step 2 - Conversation Analysis

Wireshark Conversations revealed significant communications between:

- 10.1.21.58
- 153.92.1.49

The external IP matched the IP address associated with the Lumma Stealer alert.

---

### Step 3 - DNS Analysis

The following suspicious domains were identified:

#### Domain

whitepepper.su

#### Resolved IP

153.92.1.49

---

#### Domain

whooptm.cyou

#### Resolved IP

62.72.32.156

---

### Step 4 - TLS Analysis

TLS Client Hello packets were reviewed to identify Server Name Indication (SNI) values.

Observed SNI values:

#### SNI

whitepepper.su

#### Destination IP

153.92.1.49

---

#### SNI

whooptm.cyou

#### Destination IP

62.72.32.156

---

### Step 5 - Communication Analysis

#### whitepepper.su

- IP Address: 153.92.1.49
- Data Transferred: ~2 MB
- Packets: 2,968
- Duration: ~30 seconds

This represented the most significant suspicious communication observed during the investigation.

---

#### whooptm.cyou

- IP Address: 62.72.32.156
- Data Transferred: ~12 KB
- Packets: 47
- Duration: ~32 seconds

This appeared to be a much smaller communication session.

---

### Step 6 - Host Identification

NBNS traffic identified the workstation hostname:

- DESKTOP-ES9F3ML

The same traffic was used to confirm the workstation MAC address:

- 00:21:5d:c8:0e:f2

---

### Step 7 - User Identification

Kerberos traffic revealed the user account:

- qwyatt

Using the username as a pivot point, additional Active Directory related traffic was reviewed.

SAMR traffic revealed the full user name:

- Gabriel Wyatt

---

### Step 8 - Additional Domain Activity

Following the Lumma-related communications, additional suspicious domains were observed:

- holiday-forever.cc
- communicationfirewall-security.cc

These domains warrant additional investigation due to their naming characteristics and timing within the traffic timeline.

---

## Indicators of Interest

### Victim Host

- IP Address: 10.1.21.58
- Hostname: DESKTOP-ES9F3ML
- MAC Address: 00:21:5d:c8:0e:f2
- Username: qwyatt
- Full Name: Gabriel Wyatt

---

### Suspicious Domains

- whitepepper.su
- whooptm.cyou
- holiday-forever.cc
- communicationfirewall-security.cc

---

### Suspicious IP Addresses

- 153.92.1.49
- 62.72.32.156

---

## Findings

- Host 10.1.21.58 generated the highest volume of internal traffic.
- DNS activity revealed communications with the domains whitepepper.su and whooptm.cyou.
- Whitepepper.su resolved to 153.92.1.49, the IP address referenced in the Lumma alert.
- TLS SNI values confirmed communications with both suspicious domains.
- Approximately 2 MB of traffic was exchanged with 153.92.1.49.
- NBNS traffic identified the hostname DESKTOP-ES9F3ML.
- Kerberos traffic identified the user account qwyatt.
- SAMR traffic revealed the full user name Gabriel Wyatt.
- Additional suspicious .cc domains were observed following the primary Lumma-related communications.

---

## Conclusion

The investigation identified host 10.1.21.58 (DESKTOP-ES9F3ML) as the system associated with the Lumma Stealer alert.

The affected system, used by Gabriel Wyatt (qwyatt), communicated with the suspicious domains whitepepper.su and whooptm.cyou over TLS. The most significant communication occurred with 153.92.1.49, which transferred approximately 2 MB of data and matched the alert indicator.

Additional suspicious domains observed later in the timeline may indicate further malicious activity and should be investigated separately.
