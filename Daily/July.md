---
sticker: emoji//0037-fe0f-20e3
---

## Health / Heartbeat check

- Why need this?
	- Availability Monitoring
- When need this?
	- Critical Services
	- 24/7 Operations: such as those in healthcare, financial, or e-commerce sectors.
	- **Cloud Environments**: In cloud environments, where applications are often distributed and dynamically scaled, monitoring ensures that all components are functioning correctly and that the system can adapt to changing loads.
	- **After Deployments**: Immediately following new deployments or updates, monitoring can help catch any unforeseen issues that might arise due to new changes.

- llama: Lightweight Lambda-based Application Monitoring in AWS. Tool for monitoring HTTP endpoints, ssl expiry and job freshness., using **Lambda and CloudWatch**.
	- SSL (Secure Sockets Layer) expiry refers to the expiration of an SSL certificate, a digital certificate that provides authentication for a website and enables an encrypted connection. These certificates are issued by Certificate Authorities (CAs) and are pivotal in establishing a secure connection between a client (like a web browser) and a server (where a website is hosted).

**Lambda + CloudWatch**
- EventBridge(CloudWatch Events): create a rule that triggers on schedule(using cron or rate expressions)
- Lambda: make HTTP requests to the target endpoints and handle response
- CloudWatch: set up alert based on the logs. OR configure CloudWatch Alarms to notify an SNS (Simple Notification Service) topic when an alarm state is reached. Subscribers to the SNS topic (like email addresses or SMS) can receive notifications about the alarm.

- GRPC: a modern open source high performance Remote Procedure Call (RPC) **framework** that **can run in any environment**. It enables client and server applications to communicate transparently, and makes it easier to build connected systems.


## Akamai
Content Delivery Networks (CDNs) like CloudFront. Their essential purpose is to enhance the speed, reliability, and security of delivering content over the internet by caching content at multiple edge locations around the world.


## Reflection
*Reflection: Do one thing at a time. Don't get easily swayed by others. Think through your own logic first even though it could be so wrong.* 相信自己。
遇到不懂的东西要知道它是什么，不要take it for granted.