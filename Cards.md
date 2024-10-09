---
sticker: emoji//1f4c7
---



Persist Owner Leads

When persist data a new schema, think about how will affect old data?

What about realtair? The newly added fields will affect them if we set the key to be required while its value can be null.

Realtair offers a price update to help agents send emails to consumers to see if they are interested in the house in the email. If the consumer click and open the email, the agent knows who might be interested in buying or selling a house and can follow up with them.

What does Realtair use ownerLeads data for?
For Realtair to be able to pre-populate owner leads into [Pitch](https://realtair.com/au/product/realtair-pitch/) they need to be able to pull the data from REA.

Pitch helps agents present house information beautifully, share it easily, and keep track of client interest effectively.


Resource handler returned message: "Error occurred during operation 'ECS Deployment Circuit Breaker was triggered'." (RequestToken: b5552176-384d-d59d-55ff-5ed44b6d7dd6, HandlerErrorCode: GeneralServiceException)

roll back unhealthy service deployments

the new deployment didn't pass the health checks defined in your ECS service configuration


## API Swagger Docs
Better way to write swagger docs for fp-ts

## Notifier lambda
### KMS
Create a KMS key with alias `XXX`

After we've created the KMS key, we need to use [shush](https://github.com/realestate-com-au/shush) to encrypt the secrets.
#### Encrypt

```bash

rio-as okta AWSRole shush --region ap-southeast-2 encrypt -t kms-key-alias <YOUR_SECRET_TXT>

```

#### Decrypt

```bash

rio-as okta AWSRole shush --region ap-southeast-2 decrypt <YOUR_ENCRYPTED_SECRET_TXT>

```


## Filter



