# Architecture

## Golden AMI

An AMI **with everything installed**. *(for fast deployment)*

## Elastic Beanstalk Components

To deploy en Beanstalk you should enter the following parameters:
- **Application**
- **Application version**
- **Environment** *(AWS resources)*

### Tier

Available Beanstalk tiers: 
- **Web Server Tier:** Traditionnal distribution across web instances.  
- **Worker Tier:** Multiple instances pulling from an SQS Queue.

Web Server Tier can push messages into the SQS queue of the worker environment.

### Deployment modes

Beanstalk deployment modes:
- **Single Instance:** Great for dev. Uses Elastic IP.
- **High Availability with Load Balancer**: Basic ELB / ASG pattern


