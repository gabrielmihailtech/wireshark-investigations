# Investigation 04 - It's A Trap!

## Objective

Analyze a network traffic capture associated with a fake Cloudflare verification scam and identify the affected Windows host, hostname, MAC address, and user account involved in the activity.

---

## Investigation Summary

The network capture contained traffic from a Windows workstation that interacted with external infrastructure after the user was exposed to a fake Cloudflare verification page.

The investigation focused on identifying the affected host and associated user account through network traffic analysis.

---

## Investigation Process

### Step 1 - Endpoint Analysis

Wireshark Endpoint statistics were reviewed to identify the most active internal systems.

The most active internal host was:

- IP Address: 10.6.13.133

This host generated significantly more traffic than any other internal system and became the primary focus of the investigation.

---

### Step 2 - Conversation Analysis

Wireshark Conversations identified major communications between:

- 10.6.13.133
- 83.137.149.15

Additional external communications involved:

- 104.21.112.1
- 205.174.24.80

The communication with 83.137.149.15 generated the highest amount of traffic.

---

### Step 3 - TLS Analysis

Traffic to 83.137.149.15 was reviewed.

A TLS Client Hello packet revealed:

#### SNI

```
windows.php.net
```

This confirmed the hostname contacted through the encrypted TLS session.

---

### Step 4 - Host Identification

NBNS traffic was analyzed to identify the workstation.

The following NetBIOS registration entries were observed:

```
DESKTOP-5AVE44C
```

This hostname appeared repeatedly in NetBIOS Name Service traffic.

---

### Step 5 - MAC Address Identification

After identifying the host, Ethernet II headers were reviewed.

The workstation MAC address was identified as:

```
24:77:03:ac:97:df
```

Associated vendor:

```
Intel
```

---

### Step 6 - User Identification

Kerberos authentication traffic was analyzed.

The following username was identified through the CNameString field:

```
rgaines
```

---

### Step 7 - Full Name Identification

Using the identified username, packet searches were performed using the surname:

```
Gaines
```

SAMR traffic revealed the user's full name:

```
Roman Gaines
```

---

## Indicators of Interest

### Victim Host

- IP Address: 10.6.13.133
- Hostname: DESKTOP-5AVE44C
- MAC Address: 24:77:03:ac:97:df
- Username: rgaines
- Full Name: Roman Gaines

---

### External Infrastructure

- 83.137.149.15
- 104.21.112.1
- 205.174.24.80

---

### Observed TLS Hostname

```
windows.php.net
```

---

## Findings

- Host 10.6.13.133 generated the highest volume of traffic within the network.
- NBNS traffic identified the hostname DESKTOP-5AVE44C.
- Ethernet II headers identified MAC address 24:77:03:ac:97:df.
- Kerberos traffic revealed the username rgaines.
- SAMR traffic revealed the full name Roman Gaines.
- TLS traffic contained the SNI value windows.php.net.
- Multiple external communications were observed involving the identified workstation.

---

## Conclusion

The investigation identified host 10.6.13.133 as the workstation involved in the observed activity.

Analysis of NBNS, Kerberos, SAMR, TLS, and Ethernet traffic allowed identification of the system, user account, and associated network communications.

The affected user account was identified as rgaines (Roman Gaines), and the workstation hostname was confirmed as DESKTOP-5AVE44C.
