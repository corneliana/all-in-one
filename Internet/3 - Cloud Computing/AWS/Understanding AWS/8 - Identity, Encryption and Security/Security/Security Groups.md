## Security groups
Act as a virtual firewall for your EC2 instances. Like setting up a security system for your home, deciding who can knock on the door (access your instances) and who can't.
- Another layer of abstraction over IP so that don't need to think about IPs when using groups.
- SSH

**Security Groups are stateful, so allowing inbound traffic to the necessary ports enables the connection. Network ACLs are stateless, so you must allow both inbound and outbound traffic** 