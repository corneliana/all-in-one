1. Who: Database to store data for organization, accounts, group, role, user, policy, resource. **Adding relationships between these entities in your database design. For example, many-to-many relationships between users and groups, or between policies and roles.**
2. Build concepts: What is an organization, accounts, group, role, user, policy, resource?
	1. Root user, IAM user
	2. Organization: a list of accounts, resources. Also organizational units (OUs) and policies that apply across accounts.
	3. accounts: object in organization list, also include that an account contains its own set of IAM users, roles, and resources.
	4. group: a list of users and inherit policies from attached policies
	5. role: ~~a list of users and policies~~ **identities that can be assumed by users, services, or even external entities**
3. Authorization method: user must meet a condition to access to a certain resource.
4. Event driven architecture to update database based on users' operations

---

5. Policy Evaluation: How will you evaluate policies to make access decisions?
6. Hierarchy and Inheritance: How do permissions flow from organizations to accounts to users/roles?
7. Cross-account Access: How to handle permissions across different accounts within an organization?
8. Temporary Credentials: How to generate and manage temporary security credentials, especially for roles?
9. Audit and Logging: How will you track who did what and when?
10. API and SDK: How will applications interact with your IAM system?

Authorization: determine access rights
Authentication: verify identity, e.g. passwords, multi-factor authentication, API keys, etc.


