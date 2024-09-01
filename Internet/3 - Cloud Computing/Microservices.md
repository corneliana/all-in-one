---
sticker: ui//mk-mark-blocklink
---

Microservices architecture is a method of developing software systems that structures an application as **a collection of loosely coupled services**, organized around business capabilities. Each service is a small, independent piece of the overall system that performs a specific function. This approach enables individual services to be developed, deployed, and scaled independently.

The "micro" aspect emphasizes not just the size (the granularity of the services) but the scope and bounded context of each service, aiming for simplicity and focus in service design and functionality. The key is that each service is as small as necessary but as large as required to fulfill its designated functionality effectively.

### Analogy
Imagine a busy city with various neighborhoods representing different microservices in a microservices architecture. People (requests) want to travel from one neighborhood to another to access different services. The city has a complex network of roads, and instead of each person figuring out the best route, there's a helpful guide (HTTP proxy sidecar) accompanying each person.

In this analogy:

- **Neighborhoods:** Microservices are like individual neighborhoods, each serving a specific purpose.
- **People:** Requests or users are like people trying to access services.
- **Roads:** The network connecting neighborhoods is the communication channels between microservices.

- **Guide ([[HTTP proxy side-car]]):** The guide is a knowledgeable assistant who knows the best routes, ensures safety (security features), and helps people navigate the city efficiently. This guide (proxy sidecar) takes care of directing traffic (requests) and handling any necessary tasks like security checks, load balancing, or monitoring.

So HTTP proxy sidecar acts as a helpful guide for requests, managing the journey through the city (microservices architecture), making sure they reach their destinations efficiently and safely.