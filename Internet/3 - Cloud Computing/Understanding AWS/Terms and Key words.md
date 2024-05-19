- Elastic
	- The ability to scale resources seamlessly and adapt to changing demands, whether it's storage capacity, performance, caching capacity, or IP address management.

- minimize operational overhead

- fully managed
	- E.g. Aurora, it means that AWS handles the heavy lifting of infrastructure management, but some level of user interaction and monitoring is still required to ensure the database operates effectively for the specific application needs.

- Storage v.s. memory
	- Storage is where data is kept long-term, and memory is where data is kept short-term for quick access by the computer's processor.

- **Region v.s. AZ**
	- Regions, like "ap-southeast," represent large geographic areas that contain multiple Availability Zones. Each Availability Zone within a region, such as "ap-southeast-1a," "ap-southeast-1b," etc., represents an isolated data center with its own infrastructure, facilities, and redundancy measures. Deploying resources across multiple Availability Zones within the same region ensures high availability and fault tolerance for applications by protecting against failures at the data center level.

AWS API Gateway is like a bouncer at a club, managing access to your backend services and APIs, while AWS Route 53 is like a GPS for the internet, directing traffic to your web applications using domain names.