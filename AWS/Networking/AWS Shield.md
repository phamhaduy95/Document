#### Introduction

Security service giúp ngăn chặn D-DoS attacks và bot traffic.
AWS Shield cung cấp 2 option:

1. **AWS Shield Standard** - hỗ trợ basic protection ở layer 3 và layer 4
2. **AWS Shield Advanced** - bảo vệ cho cả 3 layer (layer 3, layer 4, layer 7)

#### AWS Shield Standard

AWS Shield Standard chống đỡ các kiểu tấn công DDOS phổ biến sau

**Volumetric attacks** (**Layer 3**) These attacks aim to consume the entire bandwidth of a target system by flooding it with an overwhelming volume of traffic.

- **UDP Flood**: The attacker floods random ports on the target with UDP packets. The target's resources are exhausted as it tries to respond with "Destination Unreachable" messages.
- **ICMP Flood**: The attacker overwhelms the target with a flood of ICMP Echo Request (ping) packets, consuming bandwidth and system resources.
- **Reflection Attacks (e.g., DNS Reflection)**: Attackers send small, spoofed requests to open servers. The servers respond with much larger replies, which are sent to the target's spoofed IP, amplifying the attack's volume.

Protocol attacks (**Layer 4**): Also known as state-exhaustion attacks, these target server resources and networking equipment like firewalls by using vulnerabilities in communication protocols.

- **SYN Flood**: The attacker sends a high volume of TCP SYN (connection request) packets but never completes the handshake. This leaves the server's connection tables full of half-open connections, preventing legitimate connections.

AWS Shield standard is free.

#### AWS Shield Advanced

Application-layer attacks (Layer 7)

These attacks target vulnerabilities within the application itself to consume server resources and disrupt service.

- **HTTP Flood**: The attacker makes a flood of what appear to be legitimate HTTP requests (like GET or POST) to overwhelm web servers and tie up server resources.
- **DNS Query Flood**: The attacker floods a domain's authoritative DNS server with bogus DNS queries, exhausting the server's processing power and preventing it from responding to legitimate queries.
- **Bad bots**: Attackers use botnets to target application logic. Shield Advanced, integrated with WAF, can use rules to block malicious traffic based on behavior, IP reputation, or other characteristics.

Các lợi ích khi tham gia AWS Shield Advance

- more advanced detection
- automatic Layer 7 mitigations
- có đội ngũ chuyên gia hỗ trợ DDoS Response Team 24/7
- *cost protection* AWS Shield Advanced có dịch vụ bão lãnh đền bù cho các chi phí phát sinh do D-DoS attach cho compute service, data transfer và Route 53.
