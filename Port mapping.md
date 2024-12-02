`rii-as okta hello-world-prod-ReadWrite -- ./fargate-bastion subnet-0261234565887618b --tunnel 5005:foo-bar-api.hello-world-prod.some.com.au:80`

- `5005` is a local port number that's being used for port forwarding. This is the port that will be available on your local machine. So if you want to curl it, you curl it from a new terminal session instead of from bastion.
- `80` is the destination port number, which is the standard HTTP port on the remote server (foo-bar-api.hello-world-prod.some.com.au)

This command appears to be setting up a tunnel where:
- Any traffic you send to localhost:5005 on your machine
- Will be forwarded to port 80 on `foo-bar-api.hello-world-prod.some.com.au` through the Fargate bastion host
