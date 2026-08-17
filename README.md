Task 8: Capture Network Traffic with Wireshark
Overview
This task demonstrates the fundamentals of network traffic capture and protocol analysis using Wireshark on an Ubuntu Linux system. By monitoring local network interface activity, we filtered and isolated specific protocols (DNS, HTTP, TCP), analyzed packet structure, and investigated the security implications of unencrypted data transmission compared to secure alternatives like HTTPS.

Requirements Checklist
Environment Setup: Installed Wireshark on Ubuntu and configured non-root packet capture permissions for user mursal7x.

Traffic Capture: Captured 2+ minutes of live traffic across the local active network interface (wlan0/eth0).

Protocol Filtering & Analysis:

Applied dns display filter to observe domain name resolutions.

Applied http display filter to inspect plain-text application requests and headers.

Applied tcp display filters to isolate and verify the TCP 3-way handshake (SYN, SYN-ACK, ACK).

Artifact Export: Exported the raw capture file as wireshark_capture.pcap.

Security & Conceptual Analysis: Documented security risks associated with plain-text HTTP and created a networking glossary.

What Was Done (Step-by-Step Summary)
1. Installation & Privilege Configuration
Wireshark was installed via apt, and the current user was added to the wireshark group to allow packet capture without root/sudo access:

Bash
sudo apt update && sudo apt install wireshark -y
sudo usermod -aG wireshark $USER
newgrp wireshark
2. Live Traffic Capture & File Export
Launched Wireshark and selected the primary network interface.

Promoted background network activity (browsing web pages, DNS requests) for over 2 minutes.

Stopped the capture session and saved the raw packet stream as wireshark_capture.pcap.

3. Protocol Isolation & Inspection
DNS Inspection: Filtered using dns to trace outgoing hostname queries and returning IP mappings.

HTTP Plain-Text Inspection: Filtered using http to locate unencrypted GET requests. Inspected packet details to view clear-text HTTP headers, hostnames, and user-agent strings.

TCP Handshake Analysis: Filtered using tcp / tcp.flags.syn == 1 and isolated a single stream to observe connection establishment:

[SYN]: Client sends connection request to server.

[SYN-ACK]: Server acknowledges and responds with a connection request.

[ACK]: Client acknowledges server response, completing the connection.

Security Analysis: HTTP vs. HTTPS
Risks of Unencrypted HTTP: Standard HTTP transmits data in clear text across the wire. Anyone with access to the local network path or executing a Man-in-the-Middle (MitM) or packet sniffing attack can inspect sensitive information, including session tokens, cookies, login credentials, and personal details.

How HTTPS Prevents Eavesdropping: HTTPS secures data using Transport Layer Security (TLS/SSL). It encrypts the payload before transmission, rendering intercepted packets unreadable to unauthorized passive listeners. HTTPS also verifies server identity via digital certificates.

Networking Glossary
Packet: The basic unit of data formatted for transmission over a packet-switched network, containing control headers and a payload.

Protocol: A standardized set of rules and formats that determines how network devices communicate and exchange data.

Port: A logical software endpoint mapped to an IP address, used by operating systems to direct traffic to specific applications or services (e.g., port 80 for HTTP, port 443 for HTTPS).

Payload: The actual user data or application content carried within a packet, excluding routing headers and trailers.

Handshake: An automated negotiation protocol executed between two network endpoints to establish parameters, trust, and connection state prior to transferring data.
