## AWS Amplify
- Set of tools to get started with creating mobile and web applications:
	- Amplify Studio
	- Amplify CLI
	- Amplify Libraries
	- Amplify Hosting
- “Elastic Beanstalk for mobile and web applications”
- Must-have features such as data storage, authentication, storage, and machine-learning, all powered by AWS services
- Front-end libraries with ready-to-use components for React.js, Vue, Javascript, iOS, Android, Flutter, etc…
- Incorporates AWS best practices to for reliability, security, scalability
- Build and deploy with the Amplify CLI or Amplify Studio

### Important features
- Authentication
	- Leverages Amazon Cognito
	- User registration, authentication, account recovery & other operations
	- Support MFA, Social Sign-in, etc…
	- Pre-built UI components
	- Fine-grained authorization
- DataStore
	- Leverages Amazon AppSync and Amazon ***DynamoDB***
	- Work with local data and have auto sync to the cloud without complex code
	- Powered by GraphQL
	- Offline and real-time capabilities
	- Visual data modeling w/ Amplify Studio

### Amplify Hosting
When start deploying the application
- Build and Host Modern Web Apps
- CICD (build, test, deploy)
- Pull Request Previews
- Custom Domains
- Monitoring
- Redirect and Custom Headers
- Password protection
![[Amplify Hosting.png]]
### End-to-End (E2E) Testing
- Run end-to-end (E2E) tests in **the test phase in Amplify**
- Catch regressions before pushing code to production
- Use the test step to run any test commands at build time (amplify.yml)
- Integrated with Cypress testing framework
- Allows you to generate UI report for your tests
