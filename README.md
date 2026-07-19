# Deep in Net - Exercise Log

Small Packet Tracer exercises building network fundamentals from the ground up.

**Addressing method (used throughout):** `/n` counts network bits out of 32, not decimal digits. Where `n` isn't a multiple of 8, the split falls inside one octet. Convert that octet to 8-bit binary, split at the bit count into network | host, zero the host bits for the network address, set them all to `1` for the broadcast address; everything between is the usable range.

**Quick addressing calculation method (used throughout)**: Count the host bits as 32 − n. Each subnet contains 2^(32−n) total addresses (the block size). Find the subnet by locating the multiple of the block size that the given IP falls into. That multiple is the network address while broadcast address is network + block size − 1. The first usable host is network + 1, and the last usable host is broadcast − 1.

**OSI layers seen so far:**

| Layer | Item(s) |
|---|---|
| 1 (Physical) | Hub |
| 2 (Data Link) | Switch |
| 3 (Network) | IP |
| 4 (Transport) | TCP, UDP |
| 7 (Application) | HTTP, HTTPS, FTP, DNS, DHCP |

---

## Exercise 1 - Direct PC-to-PC links (3 separate networks)

Three isolated point-to-point networks, one cable each, no hub/switch involved: PC0-PC1, PC2-PC3, PC4-PC5.

**Key concepts**
- **RJ-45.** The 8-pin modular connector terminating each end of a twisted-pair Ethernet cable.
- **Straight-through vs. crossover.** A NIC has a fixed TX (Transmit) pin pair and a fixed RX (Receive) pin pair it never swaps roles. A *straight-through* cable wires pin-for-pin straight, so two identical devices end up TX-facing-TX and RX-facing-RX. Neither end's signal ever reaches the other's receiver. A *crossover* cable swaps the TX pair to the far end's RX pair (and vice versa), so two like devices can actually hear each other.
- **Rule of thumb:** straight-through connects unlike devices while rossover connects like devices.

**Notes**
- PC0-PC1 host `192.168.1.3/24` → network `192.168.1.0`, broadcast `192.168.1.255`, usable `.1-.254`.
- PC2-PC3 host `192.168.13.82/29` → network `192.168.13.80`, broadcast `192.168.13.87`, usable `.81-.86`.
- PC4-PC5 host `192.168.13.254/29` → network `192.168.13.248`, broadcast `192.168.13.255`, usable `.249-.254`.

---

## Exercise 2 - Hub segment vs. Switch segment

**Key concepts**
- **Hub - OSI Layer 1 (Physical).** No address awareness. It regenerates the incoming electrical signal out every other port. All 5 ports share a single collision domain so any two simultaneous transmissions corrupt each other.
- **Switch - OSI Layer 2 (Data Link).** Reads the source MAC address of each incoming frame to build a MAC address table (MAC - port). Frames to a known destination go out only that one port. Frames to an unknown destination are flooded once, then learned. Each port is effectively its own collision domain.

**Notes**
- Hub host `192.168.1.5/29` → network `192.168.1.0`, broadcast `192.168.1.7`, usable `.1-.6`.
- Switch host `192.168.1.193/27` → network `192.168.1.192`, broadcast `192.168.1.223`, usable `.193-.222`.

---

## Exercise 3 - Servers and Protocols

**Key concepts**
- **Server** is a role given to a machine to handle the requests and responses from a client. Based on the server, it can edit data, execute business logic and of course own storage.
- **DHCP (Dynamic Host Configuration Protocol)** is a server with a specific job to track which IP address is free or already given to a device, from a fixed IP pool. When a PC has no an IP to communicate with the network, it just sends a broadcast to every connected device. Then only DCHP replies to this with an IP offering. Then the pc accepts it and the server confirms.
- **DNS (Domain Name System)** is a server with a stored table of mappings Domain names to IPs or to other names that are connected to IPs. Straight mapping to IP is called `A` and mapping to another name that is mapping to an IP is called `CNAME`
- **HTTP (HyperText Transfer Protocol)** is the protocol that lets browsers communicate with both servers and clients and transfer the information using requests and responses.
- **HTTPS (HyperText Transfer Protocol Secure)** is the exact same protocol, with data encryption using a cerificate key for secured information transfer.
- **FTP (File Trasfer Protocol)** is software that allows to securely upload, download and manage files across networks.
- **TCP (Transmission Control Protocol)** is providing a realiable connection where data are being resent if lost in delivery no matter the time. **UDP (User Datagram Protocol)** is providing fast communication with no guarantee of consistent data trasfer, where its key priority is speed. Their difference is delivery verification and speed. While TCP is handling most of the webpages, UDP is ideal for streaming, video calling and where speed matters most.
- TCP and UDP are sitting on **Layer 4 of OSI model (Transport Layer)**
- A **Port** is part of the TCP/UDP header and it helps pointing data to the correct service on device, since a device can run many services and IP can only identify the device.

| Protocol | Port | OSI Layer |
|---|---|---|
| HTTP | 80 | 7 (Application) |
| HTTPS | 443 | 7 (Application) |
| FTP | 21 (control), 20 (data) | 7 (Application) |
| DNS | 53 | 7 (Application) |
| DHCP | 67 (server), 68 (client) | 7 (Application) |
| TCP | — (carries the port field for the layer above) | 4 (Transport) |
| UDP | — (carries the port field for the layer above) | 4 (Transport) |

## Exercise 4 - Routers

**Key Concepts**
- A **Router** is responsible of connecting machines from different networks using IP addresses
- The difference between a **Switch** and a **Router** is that switch is matching MAC adresses since it only connects local network machines. Router is matching whole network IPs to connect different networks.
- Since it is one step further, it also sits one layer above switch. OSI layer is **3 (Network Layer)**, where IP is sitting.
- **Default Gateway** is the IP given from each network to its connection with the router.