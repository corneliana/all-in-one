- AppSync is a managed service that uses GraphQL, making it easy to develop GraphQL APIs by handling the heavy lifting of securely connecting to data sources like DynamoDB, Lambda, and more.
- GraphQL makes it easy for applications to get exactly the data they need.
- This includes combining data from one or more sources
	- NoSQL data stores, Relational databases, HTTP APIs…
	- Integrates with DynamoDB, Aurora, OpenSearch & others
	- Custom sources with AWS Lambda
- Retrieve data in real-time with WebSocket or MQTT on WebSocket
- For mobile apps: **local data access & data synchronization**
- It all starts with uploading one GraphQL schema

## Example
![[AppSync Example.png]]

- 4 ways to authorize applications to interact with your AWS AppSync GraphQL API
	- API_KEY
	- AWS_IAM: IAM users / roles / cross-account access
	- OPENID_CONNECT: OpenID Connect provider / JSON Web Token
	- AMAZON_COGNITO_USER_POOLS
- For custom domain & HTTPS, use CloudFront in front of AppSync
