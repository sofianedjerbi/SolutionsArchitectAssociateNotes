# Architecture

## Golden AMI

An AMI **with everything installed**. (for fast boot)

## Elastic Beanstalk Components

- **Application**
- **Application version**
- **Environment** *(AWS resources)*

## Tier

- **Web Server Tier:** Traditionnal distribution across web instances.  
- **Worker Tier:** Multiple instances pulling from an SQS Queue.

Web Server Tier can push messages into the SQS queue of the worker environment.

## Deployment modes

- **Single Instance:** Great for dev. Uses Elastic IP.
- **High Availability with Load Balancer**: Basic ELB / ASG pattern


