# Investigation 01 - Portfolio ServeIRC Analysis

## Objective

Analyze network traffic associated with the domain `portfolio.serveirc.com` and identify any suspicious activity.

---

## Investigation Summary

A Windows host initiated a DNS request for `portfolio.serveirc.com`, received an IP address in response, established an HTTP connection, and downloaded a JavaScript file that appeared obfuscated.

The behavior observed during the HTTP response warranted additional investigation.

---

## Investigation Process

### Step 1 - DNS Analysis

The client system issued a DNS query for:

```text
portfolio.serveirc.com
```

The DNS server responded with:

```text
62.173.142.148
```

---

### Step 2 - TCP Connection Analysis

The client initiated a TCP connection to:

```text
62.173.142.148
```

using:

```text
Destination Port: 80
```

The following TCP three-way handshake was observed:

```text
SYN
SYN-ACK
ACK
```

---

### Step 3 - HTTP Analysis

After establishing the TCP connection, the client issued the following HTTP request:

```http
GET /login.php HTTP/1.1
```

Host:

```text
portfolio.serveirc.com
```

The server responded with:

```http
HTTP/1.1 200 OK
```

---

### Step 4 - File Delivery Analysis

The HTTP response contained the following header:

```http
Content-Disposition: attachment;
filename=allegato_708.js
```

Server:

```text
nginx/1.14.0 (Ubuntu)
```

Content-Type:

```text
application/octet-stream
```

---

### Step 5 - File Inspection

The JavaScript content delivered by the server appeared obfuscated.

Examples observed during inspection:

```javascript
String.fromCharCode(...)
_0x14b9a0
_0x363895
```

The script was not immediately human-readable and required further analysis.

---

## Indicators of Interest

### Domain

```text
portfolio.serveirc.com
```

### IP Address

```text
62.173.142.148
```

### Downloaded File

```text
allegato_708.js
```

### Requested Resource

```text
/login.php
```

---

## Findings

- The client resolved the domain portfolio.serveirc.com through DNS.
- The returned IP address was 62.173.142.148.
- The client established a TCP connection over port 80.
- An HTTP GET request was sent for /login.php.
- The server responded with HTTP 200 OK.
- The response contained a downloadable JavaScript file named allegato_708.js.
- The JavaScript content appeared obfuscated.
- No HTTP POST requests containing user credentials were identified during the investigation.

---

## Conclusion

The investigation revealed that the host accessed portfolio.serveirc.com and received a JavaScript attachment named allegato_708.js following a request to /login.php.

The downloaded JavaScript appeared obfuscated and could not be easily reviewed without additional analysis.

While the network traffic alone does not conclusively prove malicious activity, the unusual delivery of an obfuscated JavaScript file warrants further investigation.
