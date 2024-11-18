- Give users an identity to interact with our web or mobile application
- Cognito User Pools:
	- Sign in functionality for app users
	- Integrate with API Gateway & Application Load Balancer
- Cognito Identity Pools (Federated Identity):
	- Provide AWS credentials to users so they can access AWS resources directly
	- Integrate with Cognito User Pools as an identity provider
- Cognito vs IAM: “hundreds of users”, ”mobile users”, “authenticate with SAML”

## Cognito User Pools(CUP)
- Create **a serverless database of user for your web & mobile apps**
- Simple login: Username (or email) / password combination
- Password reset
- Email & Phone Number Verification
- Multi-factor authentication (MFA)
- Federated Identities: users from Facebook, Google, SAML…
- Feature: block users if their credentials are compromised elsewhere
- Login sends back a JSON Web Token (JWT)
![[Cognito User Pools.png]]
## ALB - User Authentication

## Cognito Identity Pools (Federated Identities)
- Get identities for “users” so they obtain temporary AWS credentials
- Your identity pool (e.g identity source) can include:
	- Public Providers (Login with Amazon, Facebook, Google, Apple)
	- Users in an Amazon Cognito user pool
	- OpenID Connect Providers & SAML Identity Providers
	- Developer Authenticated Identities (custom login server)
	- Cognito Identity Pools allow for unauthenticated (guest) access
- Users can then access AWS services directly or through API Gateway
	- The IAM policies applied to the credentials are defined in Cognito
	- They can be customized based on the user_id for fine grained control
![[Cognito Identity Pools.png]]

## Cognito User Pools v.s. Cognito Identity Pools

![[Cognito Identity Pools – Diagram with CUP.png]]
- Cognito User Pools (for authentication = identity verification)
	- Database of users for your web and mobile application
	- Allows to federate logins through Public Social, OIDC, SAML…
	- Can customize the hosted UI for authentication (including the logo)
	- Has triggers with AWS Lambda during the authentication flow
	- Adapt the sign-in experience to different risk levels (MFA, adaptive authentication, etc…)

- Cognito Identity Pools (for authorization = access control)
	- Obtain AWS credentials for your users
	- Users can login through Public Social, OIDC, SAML & Cognito User Pools
	- Users can be unauthenticated (guests)
	- Users are mapped to IAM roles & policies, can leverage policy variables
	
- CUP + CIP = authentication + authorization

Amazon Cognito can be used to federate mobile user accounts and provide them with their own IAM permissions, so they can be able to access their own personal space in the S3 bucket.

Amazon Cognito lets you add user sign-up, sign-in, and access control to web and mobile apps quickly and easily. It scales to millions of users and supports sign-in with social identity providers, such as Apple, Facebook, Google, and Amazon, and enterprise identity providers via SAML 2.0 and OpenID Connect.