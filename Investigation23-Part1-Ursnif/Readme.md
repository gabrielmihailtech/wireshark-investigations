# Investigation 23 – Ursnif (Gozi/ISFB) Traffic Analysis

## Objective

Analyse `2020-12-31-traffic-analysis-quiz-01.pcap`, identify the affected workstation, and document the network activity associated with the infection.

## Executive Summary

The PCAP shows suspicious activity from `10.12.19.101`, a Windows workstation identified through NBNS traffic as `DESKTOP-USER1PC`.
The workstation communicated with several external hosts, retrieved an unusual response from `metadatings.top`, and requested multiple RAR archives over HTTP.

The quiz author identifies this capture as an **Ursnif (Gozi/ISFB) infection**. The malware family name comes from the author's answer key;
the Wireshark observations below document the network evidence found during this investigation.

## Victim System

| Field | Value |
|---|---|
| IP address | `10.12.19.101` |
| Hostname | `DESKTOP-USER1PC` |
| Hostname evidence | NBNS name query sent by `10.12.19.101` |

No username was identified from the traffic examined.

## Investigation Process and Findings

### 1. Endpoint and Conversation Analysis

Wireshark's **Statistics → Endpoints** showed `10.12.19.101` as the most active internal IP address, with 4,367 packets.



**Statistics → Conversations** showed substantial communication between this workstation and `45.11.180.154`, followed by notable traffic involving `185.193.143.86` and `47.241.19.44`.

Packet counts and byte volumes helped select traffic for closer inspection; they do not establish malicious activity on their own.

### 2. DNS and TLS Activity

The workstation queried `fersite24.xyz`, which resolved to `47.241.19.44`. A subsequent TLS connection to that IP presented `fersite24.xyz` in the Server Name Indication (SNI) field. 
Application data was exchanged, but its contents were encrypted in the capture.

The workstation also queried `metadatings.top`, which resolved to `185.193.143.86`.

### 3. HTTP Activity Involving `metadatings.top`

At approximately 04:04:39 on 19 December 2020, `10.12.19.101` sent an HTTP GET request to `metadatings.top` for a resource under `/images/` with a long, encoded-looking path ending in `BsM.avi`.

The server responded with `HTTP/1.1 200 OK` and declared the content type as `text/html`. The response body appeared to contain encoded text.
Although the requested name ends in `.avi`, that extension alone does not establish that the response was a video file.

### 4. RAR Archive Requests

The workstation requested four RAR archives from `45.11.180.154` over HTTP. Each request received a `200 OK` response labelled `application/rar`.

| Approximate time | HTTP request | Destination |
|---|---|---|
| 04:15:06 | `GET /vnc32.rar` | `45.11.180.154` |
| 04:15:07 | `GET /vnc64.rar` | `45.11.180.154` |
| 04:25:06 | `GET /gr32.rar` | `45.11.180.154` |
| 04:25:08 | `GET /gr64.rar` | `45.11.180.154` |

The workstation also requested a `client.rar` resource from `176.10.118.191` at approximately 04:20:06. The server returned `200 OK` with the content type `application/x-rar-compressed`.

These responses show that archive content was served to the workstation. The PCAP analysis does not establish whether any archive was opened or its contents executed.

## Network Indicators Observed

| Indicator | Observation |
|---|---|
| `10.12.19.101` | Investigated internal workstation |
| `DESKTOP-USER1PC` | Hostname indicated by NBNS |
| `fersite24.xyz` / `47.241.19.44` | DNS resolution and TLS connection with matching SNI |
| `metadatings.top` / `185.193.143.86` | HTTP request for an unusual `/images/.../BsM.avi` resource |
| `45.11.180.154` | HTTP responses to four RAR archive requests |
| `176.10.118.191` | HTTP response to a `client.rar` request |

These indicators are specific to this training capture. Their presence alone should not be treated as proof that another system has Ursnif.

## Conclusion

The investigation identified suspicious network activity from `DESKTOP-USER1PC` (`10.12.19.101`), including a TLS connection to `fersite24.xyz`, an unusual HTTP response from `metadatings.top`, 
and several RAR archive transfers from external hosts.

The author's answer key attributes the infection to **Ursnif (Gozi/ISFB)**. The traffic analysis supports the documented connections and transfers, 
while the PCAP alone does not prove file execution or independently identify the malware family.
