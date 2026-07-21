# Wireshark Filters Cheat Sheet

## DNS Analysis

`dns`
→ Display all DNS traffic.

`dns.flags.response == 0`
→ Display DNS queries only.

`dns.flags.response == 1`
→ Display DNS responses only.

`dns.qry.name contains "domain"`
→ Search for a specific domain name.

---

## HTTP Analysis

`http`
→ Display all HTTP traffic.

`http.request`
→ Display HTTP requests only.

`http.response`
→ Display HTTP responses only.

`http.request.method == "POST"`
→ Display HTTP POST requests.

---

## IP Investigation

`ip.addr == X.X.X.X`
→ Display all traffic related to an IP address.

`ip.src == X.X.X.X`
→ Display traffic sent from a specific IP address.

`ip.dst == X.X.X.X`
→ Display traffic sent to a specific IP address.

---

## TCP Analysis

`tcp`
→ Display all TCP traffic.

`tcp.stream eq X`
→ Display a specific TCP conversation.

`tcp.flags.syn == 1`
→ Display TCP SYN packets.

`tcp.port == 80`
→ Display traffic using port 80.

`tcp.port == 443`
→ Display traffic using port 443.

---

## Authentication Analysis

`kerberos`
→ Display Kerberos traffic.

`ldap`
→ Display LDAP traffic.

`cldap`
→ Display CLDAP traffic.

---

## Keyword Searches

`frame contains "text"`
→ Search for a specific string inside packets.

`frame contains "username"`
→ Search for packets containing the word username.

`frame contains "password"`
→ Search for packets containing the word password.

`frame contains "domain"`
→ Search for packets containing the word domain.

---

## Combined Filters

`http || dns`
→ Display only HTTP and DNS traffic.

`ip.addr == X.X.X.X && http`
→ Display HTTP traffic for a specific IP address.

`ip.addr == X.X.X.X && dns`
→ Display DNS traffic for a specific IP address.

---

## Investigation Workflow

`dns`
→ Identify domains.

`ip.addr == X.X.X.X`
→ Focus on a specific host.

`http`
→ Identify web requests and downloads.

`http.request.method == "POST"`
→ Look for data sent to a server.

`kerberos`
→ Identify usernames.

`ldap`
→ Identify Active Directory information.

`tcp.stream eq X`
→ Reconstruct a specific conversation.

`Follow TCP Stream`
→ View the full conversation between client and server.
