# AWS



### Instance Types
- General Purpose — A balanced mix of CPU, memory, and storage
- Compute Optimized — Focused on compute power
- Memory Optimized — Focused on memory and large-scale enterprise-class workloads
- Accelerated Computing — Ideal for hardware acceleration (GPUs, FPGAs, etc.)
- Storage Optimized — Designed for high, sequential read/write access to large datasets
- HPC Optimized — High performance computing workloads

### Instance Purchase Options
- On-Demand Instance — Pay per hour/second without long-term commitment
- Spot Instance — Purchase unused capacity at reduced rates
- Reserved Instance — Reserve capacity with a commitment (1 or 3 years) for cost savings
- On-Demand Capacity Reservation — Reserve capacity in a specific AZ without long-term commitment


### Tenancy
- Shared Tenancy
- Dedicated Tenancy
  - Dedicated Instances
  - Dedicated Hosts
### EC2 Image Builder
- Set Up
  - Name
  - Description
  - Tags
  - Scheduler
- Recipe 
  - Pre Build
  - Create your own  
- Components
  - Build components
    - Software and Settings
  - Test components
    - Runs After image build
  - Components Document
    - yaml File
      - Code
      - Commands
      - Defintions
    - Documents phase
      - Build
      - Validate
      - Test

#### Example Document
```yaml
     phases
      -
        name: 'build'
        steps:
          -
            name: SampleS3DownLoad
            action: S3Download
            timeoutSeconds: 60
            maxAttempts:3
            onFailure: Abort
            inputs:
              - 
               source: 's3://sample-bucket/sample1.ps1'
               destination: 'C:\sample1.ps1'
              - 
               source: 's3://sample-bucket/sample2.ps1'
               destination: 'C:\sample2.ps1'
```
    

### AWS Elastic Beanstalk
- Platform as a Service (PaaS) for deploying web apps and services
- Supports multiple languages (Java, .NET, PHP, Node.js, Python, etc.)

### AWS Lambda
- Serverless compute service that runs your code in response to events
- Automatically manages compute resources and scaling

### AWS Batch
- Run batch computing workloads at scale using containers
- Efficiently schedules and executes jobs on AWS infrastructure

### Amazon Lightsail
- Simplified cloud VPS for quick deployments
- Ideal for small apps, websites, and dev/test environments

### AWS App Runner
- Deploy containerized web apps and APIs directly from source code or image
- Automatically handles scaling, load balancing, and TLS

### AWS Outposts
- Bring native AWS services to on-premises environments
- Ideal for hybrid cloud setups requiring low latency or data residency

### AWS Serverless Application Repository
- Framework for building and deploying serverless apps
- Uses AWS SAM (Serverless Application Model) or CloudFormation

### AWS SimSpace Weaver
- Simulate large-scale spatial environments in real time
- Great for training, urban planning, and event simulation


### Elastic Load Balancer (ELB)
### ELB Components
- Listener
- Target Groups
- Rules 

#### Types 
- Application Load Balancer
  - operates request level
  - sutable for application like Apachi or website hosting
- Network Load Balancer
  - Ulter high perfoemance
  - Operats connectin level routing trafic ,routing trafic to traget within your vpc
  - Handels million of traffic per second
- Classic Load Balancer
  -

