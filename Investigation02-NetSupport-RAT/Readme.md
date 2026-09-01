# Investigation 02 - NetSupport RAT Investigation

## Objective

Investigate network traffic associated with a SIEM alert related to NetSupport RAT activity and identify the affected host, user account, and indicators of compromise.

---

## Investigation Summary

A SIEM alert identified suspicious communications associated with NetSupport RAT activity involving the remote IP address:

```
45.131.214.85
```

The investigation focused on identifying the internal host responsible for the communications and collecting relevant indicators of compromise.

---

## Investigation Process

### Step 1 - Endpoint Identification

Using Wireshark Endpoints statistics, internal hosts within the following network were identified:

```
10.2.28.0/24
```

The host generating the highest amount of relevant traffic was:

```
10.2.28.88
```

---

### Step 2 - Conversation Analysis

Wireshark Conversations statistics revealed significant communications between:

```
10.2.28.88
```

and

```
45.131.214.85
```

The traffic matched the IP referenced in the SIEM alert.

---

### Step 3 - Network Traffic Analysis

After filtering traffic associated with:

```
45.131.214.85
```

multiple HTTP communications were identified.

Examples observed:

```
http
POST /fakeurl.htm
```

The communications occurred over:

```
TCP Port 443
```

However, the traffic was transmitted in clear text HTTP rather than encrypted HTTPS.

---

### Step 4 - Application Identification

HTTP headers revealed:

```
http
User-Agent: NetSupport Manager/1.3
```

Server response headers revealed:

```
http
 Server: NetSupport Gateway/1.92 (Windows NT)
```

These findings strongly correlated with the original NetSupport RAT alert.

---

### Step 5 - Host Identification

Directory-related traffic was reviewed to identify the affected workstation.

CLDAP traffic revealed:

```
DESKTOP-TEYQ2NR
```

and

```
DESKTOP-TEYQ2NR.easya123.tech
```

indicating the suspected hostname of the investigated system.

---

### Step 6 - User Identification

Kerberos authentication traffic was analyzed.

The following account was identified:

```
brolf
```

using:

```
Kerberos -> CNameString
```

---

## Indicators of Interest

### Internal Host

```
10.2.28.88
```

### Hostname

```
DESKTOP-TEYQ2NR
```

### User Account

```
brolf
```

### Remote IP Address

```
45.131.214.85
```

### User-Agent

```
NetSupport Manager/1.3
```

### Server Header

```
NetSupport Gateway/1.92 (Windows NT)
```

---

## Findings

- Host 10.2.28.88 communicated with the IP address referenced in the NetSupport RAT alert.
- The host initiated multiple HTTP POST requests.
- The communications occurred over TCP port 443.
- Traffic contained the User-Agent string "NetSupport Manager/1.3".
- Responses identified the server as "NetSupport Gateway/1.92".
- Kerberos authentication traffic identified the user account "brolf".
- CLDAP traffic identified the hostname "DESKTOP-TEYQ2NR".

---

## Conclusion

The investigation identified host 10.2.28.88 as the primary system of interest associated with the NetSupport RAT alert.

Network traffic showed repeated communications between the host and 45.131.214.85. The presence of NetSupport Manager user-agent strings and NetSupport Gateway responses strongly correlated with the alert.

Based on the observed network activity, host 10.2.28.88 should be prioritized for endpoint investigation and containment activities.

