PAPI → PAPI (Property API)
CPS API → CPS (Certificate Provisioning System) — Akamai's API for automating SSL/TLS cert issuance
GTM → yes, Global Traffic Manager — DNS-based load balancing/failover
DataStream → Akamai's real-time log streaming product (delivers logs to your SIEM)
Prolexic → yes, Akamai product for DDoS. Different from WAF: Prolexic handles network-layer volumetric floods, WAF (AAP) handles application-layer attacks
AAP → App & API Protector (Akamai's next-gen WAF replacing legacy Kona)
Broken auth → OWASP category covering session hijacking, weak passwords, etc.
7-day tuning → deploy WAF in Alert mode first (logs but doesn't block), review for false positives, then switch to Deny
Semi-dynamic content → exactly right, content that changes periodically (e.g. product pages updated hourly, cached at edge with short TTLs or cache keys)
Terraform templates → for Akamai property configuration specifically
Bot Manager behavioral signals → explained inline (mouse patterns, keystroke timing, device fingerprints) with examples of why bots fail these checks