# Day 1: Short Revision Notes

* **DNS (Domain Name System)**: The phonebook of the internet. Translates names like `google.com` into IP addresses like `142.250.190.46`.
* **TCP (Transmission Control Protocol)**: Reliable delivery of packets. Requires a 3-way handshake (`SYN` -> `SYN-ACK` -> `ACK`).
* **UDP (User Datagram Protocol)**: Unreliable, fast delivery ("fire and forget"). Good for video calls or gaming.
* **IP Address**: The street address of a server.
* **Ports**: The apartment number inside the server (e.g., 80 for HTTP, 443 for HTTPS, 5432 for Postgres).
* **127.0.0.1 (localhost)**: Internal loopback address. Only accepts traffic from inside the same machine.
* **0.0.0.0**: Listen to all network interfaces. Accepts traffic from the outside world.
* **Best Practice**: Never expose database ports to the public internet. Keep DNS TTL low before server migrations.
* **Interview cheat code**: If an API suddenly can't connect and the code didn't change, check DNS and firewalls first.
