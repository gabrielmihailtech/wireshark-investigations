# Investigation 08 - WarmCookie Malware Activity

## Objective

Investigate network traffic associated with a suspected WarmCookie malware infection and identify the affected workstation, user account, malicious infrastructure, and indicators of compromise (IOCs).

---

## Executive Summary

Network traffic analysis identified a Windows workstation within the LAFONTAINEBLEU environment that communicated with infrastructure associated with a malicious FedEx-themed lure.

The victim host accessed domains related to `checkfedexexp.com` and downloaded a ZIP archive disguised as an invoice.

HTTP traffic revealed the delivery of a file named:

```text
Invoice 876597035_003.zip
```

The observed activity is consistent with WarmCookie malware delivery through a shipping or invoice-themed social engineering lure.

---

## Victim Details

### Host Information

- IP Address: 10.8.15.133
- Domain: lafontainebleu.org
- Active Directory Environment: LAFONTAINEBLEU

### User Information

- Full Name: Pierce Lucero

### Domain Controller

- IP Address: 10.8.15.4
- Hostname: WIN-JEGJIX7Q9RS

---

## Investigation Process

### Step 1 - Endpoint Analysis

Wireshark Endpoint statistics identified:

```text
10.8.15.133
```

as the most active internal host.

The workstation generated the highest volume of network traffic and became the primary focus of the investigation.

---

### Step 2 - DNS Analysis

DNS traffic revealed normal Active Directory and Microsoft-related activity.

Further analysis identified suspicious FedEx-themed infrastructure associated with:

```text
checkfedexexp.com
```

---

### Step 3 - External Infrastructure Analysis

Review of external communications identified traffic involving:

```text
104.21.55.70
```

and

```text
172.67.170.159
```

TLS inspection revealed:

```text
SNI = business.checkfedexexp.com
```

Additional HTTP object analysis identified:

```text
quote.checkfedexexp.com
```

Both domains appeared to be part of the same malicious infrastructure.

---

### Step 4 - Malware Delivery

HTTP traffic revealed delivery of a file presented as:

```text
Invoice 876597035_003.zip
```

Observed characteristics:

```text
Content-Type: application/octet-stream
```

Size:

```text
2767 kB
```

The file was delivered through infrastructure associated with:

```text
quote.checkfedexexp.com
```

---

### Step 5 - User Identification

Directory-related traffic and account information revealed the user:

```text
Pierce Lucero
```

associated with the compromised workstation.

---

## Indicators of Compromise (IOCs)

### Victim System

```text
IP Address: 10.8.15.133
Domain: lafontainebleu.org
User: Pierce Lucero
```

---

### Domains

```text
business.checkfedexexp.com

quote.checkfedexexp.com
```

---

### External IP Addresses

```text
104.21.55.70

172.67.170.159

72.5.43.29
```

---

### Malware Delivery File

```text
Invoice 876597035_003.zip
```

---

### HTTP Characteristics

```text
Content-Type: application/octet-stream
```

Size:

```text
2767 kB
```

---

## Findings

- Host 10.8.15.133 generated the largest amount of network traffic in the capture.
- DNS, HTTP, and TLS traffic revealed communications with FedEx-themed domains associated with checkfedexexp.com.
- TLS traffic identified the domain business.checkfedexexp.com.
- HTTP object analysis identified quote.checkfedexexp.com.
- A ZIP archive named Invoice 876597035_003.zip was delivered to the workstation.
- The downloaded file was transferred as application/octet-stream content.
- User information identified Pierce Lucero as the affected individual.
- The observed activity is consistent with WarmCookie malware delivery through a socially engineered invoice lure.

---

## Conclusion

The investigation identified workstation 10.8.15.133 within the LAFONTAINEBLEU environment as the likely compromised host.

The victim accessed malicious infrastructure using FedEx-themed domains associated with checkfedexexp.com and downloaded a ZIP archive named Invoice 876597035_003.zip.

The evidence indicates a malware delivery chain consistent with WarmCookie infection activity. 
The identified domains, IP addresses, and downloaded file should be treated as indicators of compromise and incorporated into detection and containment efforts.
