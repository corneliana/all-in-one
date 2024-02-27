---
sticker: lucide//spline
---
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

- Hypertext Transfer Protocol (HTTP): Esperanto that everyone can understand so as to enable seamless communication, the foundation of any data exchange on the Web.
	- 比如，成语就是中国人的超链接. But now server could send any content type to client, so “Hyper Text” became misnomer. HMTP or Hypermedia might have made more sense.
	- a TCP/IP based application layer communication protocol to standardize how hosts (clients and servers) communicate. 
	- a stateless protocol, so each command runs independent of any other command, and the server does not keep any data (state) between two requests. In the original spec, HTTP requests each created and closed a TCP connection. In newer versions of the HTTP protocol (HTTP 1.1 and above), persistent connection allows for multiple HTTP requests to pass over a persistent TCP connection, improving resource consumption. In the context of DoS or DDoS attacks, HTTP requests in large quantities can be used to mount an attack on a target device, and are considered part of application layer attacks or layer 7 attacks.
	- follows the “Client-Server model” with a client opening a connection request, then waiting until it receives a response. 
	- a typical HTTP request contains
		- HTTP version type
		- a URL
		- an HTTP method
		- HTTP request headers: communicate core information, such as what browser the client is using and what data is being requested.
		- Optional HTTP body: contains the ‘body’ of information the request is transferring
	- a typical HTTP response contains
		- an HTTP status code: 3-digit codes most often used to indicate whether an HTTP request has been successfully completed.
		- HTTP response headers: convey important information such as the language and format of the data being sent in the response body.
		- optional HTTP body: contains the requested information. In most web requests, it is HTML data that a web browser will translate into a webpage.
		- By default, TCP port 80 is used, but other ports can also be used. HTTPS, however, uses port 443. HTTPS: An encrypted version of HTTP for privacy(none can eavesdrop the message), integrity(avoid manipulation) and identification(a digital signature attached to a message can identify the sender).
	- History
		- HTTP/1.0- 1996: connectionless, couldn’t have multiple requests per connection. Whenever a client will need something from the server, it will have to open a new TCP connection and after that single request has been fulfilled, connection will be closed. This large number of connections results in a serious performance hit as requiring a new TCP connection imposes a significant performance penalty because of three-way handshake followed by slow-start.
		- HTTP/1.1 - 1997: persist connections, i.e. connections weren’t closed by default and were kept open to allow multiple sequential requests. Clients usually send this header in the last request to safely close the connection. But when is the last and when to start next? => Content-Length header: present which clients can use to identify where the response ends and it can start waiting for the next response. Use Chunked Transfers In case of dynamic content, server may start sending the content in pieces (chunk by chunk) and add the Content-Length for each chunk when it is sent if fails to find out the Content-Length when the transmission starts. In order to notify the client about the chunked transfer, server includes the header Transfer-Encoding: chunked.
		- SPDY - 2009: didn’t try to replace HTTP; it was a translation layer over HTTP existing at the application layer and modified the request before sending it over to the wire. It started to become a defacto standards and majority of browsers started implementing it.
		- HTTP/2 - 2015: designed for low latency transport of content. The major building blocks are Frames and Streams: Frames are nothing but binary pieces of data. A collection of frames is called a Stream.
		- Binary instead of Textual
		- Multiplexing - Multiple asynchronous HTTP requests over a single connection
		- Header compression using HPACK
		- Server Push - Multiple responses for single request
		- Request Prioritization: Client can send a PRIORITY frame to change the priority of a stream. Without any priority information, server processes the requests asynchronously i.e. without any order. If there is priority assigned to a stream, then based on this prioritization information, server decides how much of the resources need to be given to process which request.
	- Security
	
	- FTP (File Transfer Protocol)
	- SMTP (Simple Mail Transfer Protocol)

😶‍🌫️: how these technologies and protocols work together to enable communication and data exchange? 


  





  
