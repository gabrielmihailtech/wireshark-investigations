# Investigation 05 - Fake Google Authenticator Malware Infection

## Objective

Investigate a malware infection originating from a fake Google Authenticator website and identify the affected workstation, user account, malicious domains, downloaded payloads, and command-and-control (C2) infrastructure.

---

## Investigation Summary

The investigated traffic revealed a Windows workstation that accessed a fake Google Authenticator website and subsequently downloaded malicious content.

The infection chain involved a fake software download page, PowerShell payloads, additional file downloads, and communications with multiple command-and-control (C2) servers.

---

## Investigation Process

### Step 1 - Endpoint Analysis

Wireshark Endpoint statistics identified the most active internal host:

- IP Address: 10.1.17.215

This system generated the highest volume of traffic and became the primary subject of the investigation.

---

### Step 2 - Conversation Analysis

Wireshark Conversations statistics identified several significant external communications involving:

- 5.252.153.241
- 45.125.66.32
- 45.125.66.252

Further investigation revealed these IP addresses were associated with post-infection activity.

---

### Step 3 - Victim Identification

The infected system was identified as:

- IP Address: 10.1.17.215
- Hostname: DESKTOP-L8C5GSJ

NBNS traffic confirmed the workstation hostname.

---

### Step 4 - MAC Address Identification

Ethernet II headers were reviewed to identify the workstation hardware address.

The infected workstation used:

- 00:d0:b7:26:4a:74

---

### Step 5 - User Identification

Kerberos authentication traffic revealed the user account:

- shutchenson

---

### Step 6 - Full Name Identification

Further analysis of SAMR traffic revealed the associated user:

- Steve Hutchenson

---

### Step 7 - Initial Access Investigation

DNS, HTTP, and TLS traffic identified activity associated with a fake Google Authenticator software page.

Observed suspicious domains included:

- google-authenticator.burleson-appliance.net
- authenticatoor.org

The domain `authenticatoor.org` was determined to be the fake software site used for the initial malware download.

---

### Step 8 - Payload Delivery

HTTP traffic revealed the download of multiple suspicious files.

Examples included:

- application_setup
- 29842.ps1
- pas.ps1
- TeamViewer
- TeamViewer_Resource_fr

The presence of PowerShell scripts and remote-access software strongly suggested malicious post-infection activity.

---

### Step 9 - Command and Control Analysis

The infected host communicated with the following external systems after the initial compromise:

- 5.252.153.241
- 45.125.66.32
- 45.125.66.252

Observed traffic included:

```http
GET /api/file/get-file/...
```

and recurring HTTP requests consistent with malware beaconing and task retrieval.

---

## Indicators of Compromise (IOCs)

### Victim Details

- IP Address: 10.1.17.215
- Hostname: DESKTOP-L8C5GSJ
- MAC Address: 00:d0:b7:26:4a:74
- Username: shutchenson
- Full Name: Steve Hutchenson

---

### Malicious Domains

- authenticatoor.org
- google-authenticator.burleson-appliance.net

---

### Command and Control Infrastructure

- 5.252.153.241
- 45.125.66.32
- 45.125.66.252

---

### Suspicious Downloads

- application_setup
- 29842.ps1
- pas.ps1
- TeamViewer
- TeamViewer_Resource_fr

---

## Findings

- Host 10.1.17.215 was identified as the infected Windows workstation.
- The workstation user was identified as Steve Hutchenson (shutchenson).
- The user accessed a fake Google Authenticator website.
- The domain authenticatoor.org was identified as the likely malware delivery site.
- HTTP traffic revealed downloads of PowerShell scripts and additional files.
- Post-infection activity involved communications with multiple command-and-control servers.
- The observed activity is consistent with a malware infection resulting from a fake software download.

---

## Conclusion

The investigation determined that workstation 10.1.17.215 (DESKTOP-L8C5GSJ) was infected after accessing a fake Google Authenticator website.

The malicious domain authenticatoor.org facilitated the initial infection, which led to the download of PowerShell-based payloads and subsequent communications with multiple command-and-control servers.

The affected user was identified as Steve Hutchenson, and the infected host established communications with 5.252.153.241, 45.125.66.32, and 45.125.66.252 during post-infection activity.

The identified domains, IP addresses, and downloaded files should be considered Indicators of Compromise and used for detection and blocking activities.
