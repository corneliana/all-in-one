---
sticker: lucide//spline
---
The knowledge map of Internet:
- [[Infrastructure and Administration]]
- [[All in One/Internet/1 - Protocols and Standards/AAA Overview]]
- [[All in One/Internet/2 - Web Development/AAA Overview]]
- [[Security]]
- [[Cloud Computing]]
- [[Diagnostics and Troubleshooting]]
The categorization reflects the layered and interconnected nature of the Internet. From the underlying protocols that enable communication to the cloud platforms that extend its capabilities, each aspect contributes to the functionality, security, and resilience of the global Internet so as to understand how the Internet operates, how it is managed, and how it can be protected and optimized.

# How does the Internet work?
## What is Internet? And Network? Web?

- Network is a general concept, imagine it as a wire to connect two dots.
- Internet is a network of networks, a specific type of network, and a technical infrastructure connecting billions of computers.
- Web is a service built upon the internet infrastructure. It's one of many services available on the internet, alongside others like email (using SMTP for sending messages) and IRC (Internet Relay Chat for live messaging)

The essence of how the internet works involves uniquely identifying devices (via IP addresses), connecting them through various types of networks (like LANs, WANs), and transmitting data between them following specific protocols (TCP/IP).

## identify devices by IP address

IP address is to identify devices on a network.
Compared with MAC address(hardware-specific), IP makes it easier to group subnets so that the source and target IP addresses remain unchanged during data transmission, making it faster.
- **ping** and **telnet** to test internet connection
	- ping: The term "ping" stands for Packet Internet Groper. It is a fundamental TCP/IP command used to test the reachability of a host on an Internet Protocol (IP) network. Ping operates by sending an ICMP (Internet Control Message Protocol) Echo Request to a target host and waits for an ICMP Echo Reply. This tool helps in troubleshooting connectivity, reachability, and name resolution issues.
	- telnet: a network protocol used to provide a command-line interface for communication with a remote device or server. It can be used to test connectivity to specific services running on a host by connecting to the respective port.

Under the same IP, there can be multiple ports to identify multiple services or applications running in this device. In a sense, IP is the main address while ports refer to its sub-addresses.
*Imagine a country's addresses and the addresses of its cities in a map.*

IP address ➕ a port number => socket to represent a specific endpoint for communication.
However, humans need a more readable identifier for IP addresses —— DNS: translate domain names (a human-readable name) into IP addresses. Phonebook of the Internet.

## connect devices by wire

Connection is established between two sockets, and the devices negotiate various parameters such as the maximum segment size and window size, to determine how data will be transmitted over the connection. Yet building direct connection between two devices is neither practical nor feasible, so we use an intermediary: hub => switch => router.

## transmit data in packets through router

Data is typically transmitted in segments for performance, reliability and scalability. Each segment contains a sequence number and other metadata to ensure reliable delivery.
- router: a signaler at a railway station, making sure that a message sent from a given computer arrives at the right destination computer. Wrap and unwrap layers to find out next destination.
- modem: modulate and demodulate signals to enable the transfer of data between digital devices and across networks.
- Internet Service Provider (ISP): a company to manage special routers that are all linked together and can also access other ISPs' routers.
- TCP/IP: underlying communication protocol used by most internet-based applications and services.

### in a reliable and secure manner

- Transmission Control Protocol (TCP): ensures packets are transmitted reliably and in the correct order. v.s. User Datagram Protocol (UDP)
- Secure Sockets Layer/Transport Layer Security (SSL/TLS): secure communication over the internet by establishing trust between client and server.
- Certificates: contain information about the identity of server and are signed by a trusted third party (Certificate Authority) to verify authenticity. Ensure to obtain and maintain valid SSL/TLS certificates for servers.
- Handshake: client and server exchange info to negotiate encryption algorithm and parameters for secure connection.
- Encryption: Once secure connection is established, data is encrypted using the agreed-upon algorithm and can be transmitted securely between client and server.

### and it must be standardized for scalability, efficiency, reliability

- HTTP: Esperanto that everyone can understand so as to enable seamless communication, the foundation of any data exchange on the Web.
- FTP (File Transfer Protocol)
- SMTP (Simple Mail Transfer Protocol)

😶‍🌫️: how these technologies and protocols work together to enable communication and data exchange? 


  








