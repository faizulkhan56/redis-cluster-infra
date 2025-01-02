# Redis Cluster Infrastructure

## Overview

This project sets up a **Redis Cluster** infrastructure using [Pulumi](https://www.pulumi.com/), an infrastructure as code tool. The setup includes:

- **Provisioning AWS EC2 Instances**: Deploying instances to host the Redis nodes.
- **Configuring Security Groups**: Managing firewall rules to control traffic.
- **Setting Up Redis Cluster**: Installing and configuring Redis in cluster mode across the instances.

## Prerequisites

Before you begin, ensure you have the following installed:

- [Node.js](https://nodejs.org/) (v14 or higher)
- [Pulumi CLI](https://www.pulumi.com/docs/get-started/install/)
- [AWS CLI](https://aws.amazon.com/cli/) configured with appropriate credentials
- [Redis CLI](https://redis.io/topics/rediscli) for interacting with the Redis cluster

## Project Structure

```plaintext
redis-cluster-infra/
├── index.js               # Pulumi program defining the infrastructure
├── package.json           # Node.js project metadata
├── package-lock.json      # Dependency lock file
├── Pulumi.yaml            # Pulumi project configuration
├── Pulumi.dev.yaml        # Pulumi stack configuration for the 'dev' environment
├── MyKeyPair.pem          # SSH key pair for accessing EC2 instances
└── README.md              # Project documentation
```

## Setup Instructions

### 1. Install Dependencies

Navigate to the project directory and install the required Node.js packages:

```bash
npm install
```

### 2. Configure AWS Credentials

Ensure your AWS CLI is configured with the necessary credentials:

```bash
aws configure
```

### 3. Initialize Pulumi

Initialize the Pulumi stack:

```bash
pulumi stack init dev
```

### 4. Set Configuration Values

Set the required configuration values for your Pulumi stack:

```bash
pulumi config set aws:region us-east-1
pulumi config set redisCluster:nodeCount 6
```

### 5. Deploy the Infrastructure

Deploy the infrastructure using Pulumi:

```bash
pulumi up
```

Review the proposed changes and confirm to proceed.

### 6. Access the Redis Cluster

After deployment, retrieve the public IP addresses of the EC2 instances from the Pulumi outputs:

```bash
pulumi stack output
```

Use the `redis-cli` to connect to the Redis cluster:

```bash
redis-cli -c -h <EC2_INSTANCE_IP> -p 6379
```

Replace `<EC2_INSTANCE_IP>` with the actual IP address of one of your Redis nodes.

## Security Considerations

- **SSH Key Management**: The `MyKeyPair.pem` file is used for SSH access to the EC2 instances. Ensure it is securely stored and has appropriate permissions:

  ```bash
  chmod 400 MyKeyPair.pem
  ```

- **Firewall Rules**: Security groups are configured to allow necessary traffic. Review and modify them as per your security requirements.

## Cleanup

To destroy the deployed infrastructure and avoid incurring charges:

```bash
pulumi destroy
```

Confirm the destruction when prompted.

## References

- [Pulumi AWS Documentation](https://www.pulumi.com/docs/intro/cloud-providers/aws/)
- [Redis Cluster Tutorial](https://redis.io/topics/cluster-tutorial)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
