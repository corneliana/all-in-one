Deploy and manage applications in the cloud without worrying about the infrastructure.

## Typical Architecture: Web App 3-tier
```mermaid
graph LR
    %% Entities
    User(("User"))
    Route53["Route 53"]
    ELB["ELB<br/>Public Subnet"]
    
    %% Auto Scaling Group with 3 M5 instances
    subgraph ASG["Auto Scaling Group (Private Subnet)"]
        M5_1["M5 Instance<br/>AZ 1"]
        M5_2["M5 Instance<br/>AZ 2"]
        M5_3["M5 Instance<br/>AZ 3"]
    end
    
    %% Data Subnet Services
    subgraph DATA["Data Subnet"]
        Cache["ElastiCache<br/>Session/Cache Data"]
        RDS["Amazon RDS<br/>Read/Write Data"]
    end
    
    %% Connections - Modified to show sequential flow
    User --> Route53
    Route53 --> ELB
    ELB --> M5_1
    ELB --> M5_2
    ELB --> M5_3
    M5_1 --> Cache
    M5_1 --> RDS
    M5_2 --> Cache
    M5_2 --> RDS
    M5_3 --> Cache
    M5_3 --> RDS

    %% Styling
    classDef user fill:#f9f,stroke:#333,stroke-width:2px
    classDef route53 fill:#9370db,stroke:#333,stroke-width:2px
    classDef elb fill:#ff8c00,stroke:#333,stroke-width:2px
    classDef instance fill:#ff8c00,stroke:#333,stroke-width:2px
    classDef data fill:#4169e1,stroke:#333,stroke-width:2px

    class User user
    class Route53 route53
    class ELB elb
    class M5_1,M5_2,M5_3 instance
    class Cache,RDS data
```
**Developer problems on AWS**
- Managing infrastructure
- Deploying Code
- Configuring all the databases, load balancers, etc
- Scaling concerns
- Most web apps have the same architecture (ALB + ASG)
- All the developers want is for their code to run!
- Possibly, consistently across different applications and environments

## Overview of Beanstalk
- Elastic Beanstalk is a developer centric view of deploying an application on AWS
- It uses all the component’s we’ve seen before: EC2, ASG, ELB, RDS, …
- Managed service, and devs just need to manage the application code
- Automatically handles capacity provisioning, load balancing, scaling, application health monitoring, instance configuration, …
- We still have full control over the configuration
- Beanstalk is free but you pay for the underlying instances

## Components
- Application: collection of Elastic Beanstalk components (env, versions, configurations, …)
- Application Version: an iteration of your application code
- Environment
	- Collection of AWS resources running an app version (only one app version at a time)
	- Tiers: Web Server Environment Tier & Worker Environment Tier
	- Can create multiple environments (dev, test, prod, …)

```mermaid
graph LR
    Create["Create<br/>Application"] --> Upload["Upload<br/>Version"]
    Upload --> Launch["Launch<br/>Environment"]
    Launch --> Manage["Manage<br/>Environment"]
    Manage -->|deploy new version| Upload
    Upload -->|update version| Manage

    %% Styling
    classDef default fill:#4682B4,stroke:#333,color:white,stroke-width:2px
```

## Supported Platforms
- Go, Java SE, Java with Tomcat, .NET Core on Linux, .NET on Windows Server, Node.js, PHP, Python, Ruby, Packer Builder, Single Container Docker, Multi-container Docker, Preconfigured Docker

## Web Server Tier vs. Worker Tier
- Web Server Tier: Client -> ELB -> Web Servers (responds immediately)
	- Handles incoming HTTP(S) requests directly from clients
	- Typically runs web applications that need to respond immediately to user requests
	- Automatically includes an ELB and Auto Scaling Group
	- Use cases:
		- Web fronted serving pages
		- REST APIs responding to client requests
		- User Interface components
```mermaid
graph TD
    subgraph WebEnv["Web Environment (myapp.us-east-1.elasticbeanstalk.com)"]
        ELB["ELB"]
        subgraph AZones["Availability Zones"]
            direction LR
            subgraph AZ1["Availability Zone 1"]
                SG1["Security Group"]
                subgraph ASG1["Auto Scaling Group"]
                    EC2_1["EC2 Instance<br/>(Web Server)"]
                end
                SG1 --> EC2_1
            end
            subgraph AZ2["Availability Zone 2"]
                SG2["Security Group"]
                subgraph ASG2["Auto Scaling Group"]
                    EC2_2["EC2 Instance<br/>(Web Server)"]
                end
                SG2 --> EC2_2
            end
        end
        ELB --> EC2_1
        ELB --> EC2_2
    end

    classDef az fill:#e6f3ff,stroke:#666
    classDef elb fill:#9370db,stroke:#333
    classDef sg fill:#ff6b6b,stroke:#333
    classDef ec2 fill:#ff8c00,stroke:#333
    classDef asg fill:#ffa07a,stroke:#333,stroke-dasharray: 5

    class AZ1,AZ2 az
    class ELB elb
    class SG1,SG2 sg
    class EC2_1,EC2_2 ec2
    class ASG1,ASG2 asg
```

- Worker Tier: Web Server -> SQS Queue -> Worker (processes in background)
	- Scale based on the number of SQS messages
	- Can push messages to SQS queue from another Web Server Tier
	- Handles background processing tasks that can take a long time
	- Instead of HTTP requests, it processes messages from an SQS queue
	- Designed for asynchronous workloads
	- Examples:
	    - Video encoding
	    - Image processing
	    - Email sending
	    - Report generation
	    - Data processing
	    - Long-running calculations
```mermaid
graph TD
    subgraph WorkerEnv["Worker Environment"]
        SQS["SQS Queue"]
        subgraph AZones["Availability Zones"]
            direction LR
            subgraph AZ1["Availability Zone 1"]
                MSG1["SQS message"]
                subgraph ASG1["Auto Scaling Group"]
                    EC2W1["EC2 Instance<br/>(Worker)"]
                end
                MSG1 --> EC2W1
            end
            subgraph AZ2["Availability Zone 2"]
                MSG2["SQS message"]
                subgraph ASG2["Auto Scaling Group"]
                    EC2W2["EC2 Instance<br/>(Worker)"]
                end
                MSG2 --> EC2W2
            end
        end
        SQS --> MSG1
        SQS --> MSG2
    end

    classDef az fill:#e6f3ff,stroke:#666
    classDef sqs fill:#ff69b4,stroke:#333
    classDef msg fill:#ff69b4,stroke:#333,stroke-dasharray: 5
    classDef ec2 fill:#ff8c00,stroke:#333
    classDef asg fill:#ffa07a,stroke:#333,stroke-dasharray: 5

    class AZ1,AZ2 az
    class SQS sqs
    class MSG1,MSG2 msg
    class EC2W1,EC2W2 ec2
    class ASG1,ASG2 asg
```

## Deployment Modes
- Single Instance - great for dev
```mermaid
graph TD
    subgraph Development["Single Instance Mode (Dev)"]
        EIP["Elastic IP"]
        EC2["EC2 Instance"]
        RDS["RDS Master"]
        
        EIP --> EC2
        EC2 --> RDS
    end

    classDef lb fill:#9370db,stroke:#333,stroke-width:2px
    classDef instance fill:#ff8c00,stroke:#333,stroke-width:2px
    classDef db fill:#4169e1,stroke:#333,stroke-width:2px
    classDef ip fill:#ffA500,stroke:#333,stroke-width:2px

    class EC2 instance
    class RDS db
    class EIP ip
```


- High Availability with Load Balancer - great for prod
```mermaid
graph TD
    subgraph Production["High Availability Mode (Prod)"]
        ALB["Application Load Balancer"]
        subgraph ASG["Auto Scaling Group"]
            EC2_1["EC2 Instance<br/>Zone 1"]
            EC2_2["EC2 Instance<br/>Zone 2"]
        end
        RDS_M["RDS Master<br/>Zone 1"]
        RDS_S["RDS Standby<br/>Zone 2"]
        
        ALB --> EC2_1
        ALB --> EC2_2
        EC2_1 --> RDS_M
        EC2_2 --> RDS_S
    end

    classDef lb fill:#9370db,stroke:#333,stroke-width:2px
    classDef instance fill:#ff8c00,stroke:#333,stroke-width:2px
    classDef db fill:#4169e1,stroke:#333,stroke-width:2px

    class ALB lb
    class EC2_1,EC2_2 instance
    class RDS_M,RDS_S db
```

- Why These Modes?
	1. Cost vs Reliability Trade-off:
	    - Dev envs don't need high availability, so single instance is more cost-effective
	    - Prod needs reliability, justifying the higher cost of HA setup
	2. Different Requirements:
	    - Development: Quick iteration, easy debugging
	    - Production: Stability, scalability, redundancy
	3. Resource Optimization:
	    - Dev can use smaller instances
	    - Production needs capacity for real user loads
	4. Risk Management:
		- Dev can tolerate some downtime
	    - Production requires minimal downtime and data loss protection

## Options for Updates
- All at once (deploy all in one go) – fastest, but instances aren’t available to serve traffic for a bit (downtime)
- Rolling: update a few instances at a time (bucket), and then move onto the next bucket once the first bucket is healthy
- Rolling with additional batches: like rolling, but spins up new instances to move the batch (so that the old application is still available)
- Immutable: spins up new instances in a new ASG, deploys version to these instances, and then swaps all the instances when everything is healthy. 
	- In this mode, a full set of new instances running the new version of the application in a separate Auto Scaling Group is launched. To roll back quickly, this mode terminates the ASG holding the new application version, while the current one is untouched and already running at full capacity.
- Blue Green: create a new environment and switch over when ready
- Traffic Splitting: canary testing – send a small % of traffic to new deployment

**Immutable deployments perform an immutable update to launch a full set of new instances running the new version of the application in a separate Auto Scaling group, alongside the instances running the old version. Immutable deployments can prevent issues caused by partially completed rolling deployments.**

Traffic-splitting deployments let you perform canary testing as part of application deployment. In a traffic-splitting deployment, Elastic Beanstalk launches a full set of new instances just like during an immutable deployment. It then forwards a specified percentage of incoming client traffic to the new application version for a specified evaluation period.

Some policies replace all instances during the deployment or update. This causes all accumulated Amazon EC2 burst balances to be lost. It happens in the following cases:
1. Managed platform updates with instance replacement enabled
2. Immutable updates
3. Deployments with immutable updates or traffic splitting enabled

## Deployment Process
- Describe dependencies
- Package code as zip, and describe dependencies
	- Python: requirements.txt
	- Node.js: package.json
- Console: upload zip file (creates new app version), and then deploy
- CLI: create new app version using CLI (uploads zip), and then deploy
- Elastic Beanstalk will deploy the zip on each EC2 instance, resolve dependencies and start the application

## Lifecycle policy
- Elastic Beanstalk can store at most 1000 application versions
- If you don’t remove old versions, you won’t be able to deploy anymore
- To phase out old application versions, use a lifecycle policy
	- Based on time (old versions are removed)
	- Based on space (when you have too many versions)
- Versions that are currently used won’t be deleted
- Option not to delete the source bundle in S3 to prevent data loss

## Extensions
- A zip file containing our code must be deployed to Elastic Beanstalk
- All the parameters set in the UI can be configured with code using files
- Requirements:
	- in the `.ebextensions/` directory in the root of source code
	- YAML / JSON format
	- .config extensions (example: logging.config)
	- Able to modify some default settings using: option_settings
	- Ability to add resources such as RDS, ElastiCache, DynamoDB, etc…
- Resources managed by `.ebextensions` get deleted if the environment goes away

## Under the hood
- Under the hood, Elastic Beanstalk relies on CloudFormation
- CloudFormation is used to provision other AWS services
- Use case: you can define CloudFormation resources in `.ebextensions` to provision ElastiCache, an S3 bucket, anything you want!

## Beanstalk Cloning
- Clone an environment with the exact same configuration
- Useful for deploying a “test” version of the application
- All resources and configuration are preserved, including:
	- Load Balancer type and configuration
	- RDS database type (but the data is not preserved)
	- Environment variables
- After cloning an environment, you can change its settings

## Beanstalk Migrations
- After creating an Elastic Beanstalk environment, cannot change the Elastic Load Balancer type but only its configurations, so to migrate:
	1. create a new environment with the same configuration except LB(can’t clone)
	2. deploy the application onto the new environment
	3. perform a CNAME swap or Route 53 update

- For RDS
	- RDS can be provisioned with Beanstalk, which is great for dev / test
	- This is not great for prod as the database lifecycle is tied to the Beanstalk environment lifecycle. So the best for prod is to: 
		- separately create an RDS database and provide our EB application with the connection string
		- decouple RDS
			1. create a snapshot of RDS (as a safeguard)
			2. Go to the RDS console and protect the RDS database from deletion
			3. Create a new Elastic Beanstalk env, without RDS, point the app to existing RDS
			4. perform a CNAME swap (blue/green) or Route 53 update, confirm working
			5. Terminate the old environment (RDS won’t be deleted)
			6. Delete CloudFormation stack (in DELTE_FAILED state)
	
