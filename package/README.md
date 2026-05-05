# community/registry-aws

AWS ECR (Elastic Container Registry) integration for the aux4 registry system. This package provides commands to authenticate with ECR, push Docker images to ECR repositories, and create new ECR repositories.

## Prerequisites

- **AWS CLI** must be installed and configured with valid credentials (`aws configure`).
- **Docker** must be installed and running.
- An AWS account with ECR permissions.

## Installation

```bash
aux4 aux4 pkger install community/registry-aws
```

## System Dependencies

This package requires the AWS CLI. If it is not installed, aux4 will attempt to install it using one of the following system installers:

- [brew](/r/public/packages/aux4/system-installer-brew) (macOS): `awscli`
- Linux package managers: `awscli`

For more details, see [system-installer](/r/public/packages/aux4/pkger/commands/aux4/pkger/system).

## Quick Start

Authenticate with ECR and push a local Docker image:

```bash
# Login to ECR
aux4 registry aws login --accountId 123456789012

# Push an image
aux4 registry aws push myapp:v1.0.0 --repository myapp --imageTag v1.0.0 --accountId 123456789012
```

## Environment Variables

You can set these environment variables to avoid passing flags on every command:

- `AWS_REGION` -- AWS region (default: `us-east-1`)
- `AWS_ACCOUNT_ID` -- AWS account ID
- `AWS_PROFILE` -- AWS CLI profile name

```bash
export AWS_REGION=us-west-2
export AWS_ACCOUNT_ID=123456789012

# Now you can omit --region and --accountId
aux4 registry aws login
aux4 registry aws push myapp:v1.0.0 --repository myapp
```

## Commands

### registry aws login

Authenticate with AWS ECR. This obtains a login password from ECR using the AWS CLI and passes it to `docker login`.

```bash
aux4 registry aws login --accountId 123456789012
aux4 registry aws login --accountId 123456789012 --region us-west-2
aux4 registry aws login --accountId 123456789012 --profile production
```

| Flag | Description | Default | Env |
|------|-------------|---------|-----|
| `--region` | AWS region | `us-east-1` | `AWS_REGION` |
| `--accountId` | AWS account ID | -- | `AWS_ACCOUNT_ID` |
| `--profile` | AWS CLI profile | -- | `AWS_PROFILE` |

### registry aws push

Tag and push a local Docker image to an ECR repository. This command automatically logs in to ECR before pushing.

```bash
aux4 registry aws push myapp:v1.0.0 --repository myapp --imageTag v1.0.0 --accountId 123456789012
aux4 registry aws push myapp:latest --repository myapp --accountId 123456789012
```

The image argument is positional. If `--imageTag` is not specified, it defaults to `latest`.

| Flag | Description | Default | Env |
|------|-------------|---------|-----|
| `image` | The local image to push (positional) | -- | -- |
| `--repository` | The ECR repository name | -- | -- |
| `--imageTag` | The image tag | `latest` | -- |
| `--region` | AWS region | `us-east-1` | `AWS_REGION` |
| `--accountId` | AWS account ID | -- | `AWS_ACCOUNT_ID` |
| `--profile` | AWS CLI profile | -- | `AWS_PROFILE` |

### registry aws create

Create an ECR repository if it does not already exist. If the repository already exists, the command succeeds without making changes.

```bash
aux4 registry aws create myapp
aux4 registry aws create myapp --region us-west-2
```

The repository name is a positional argument.

| Flag | Description | Default | Env |
|------|-------------|---------|-----|
| `repository` | The ECR repository name (positional) | -- | -- |
| `--region` | AWS region | `us-east-1` | `AWS_REGION` |
| `--profile` | AWS CLI profile | -- | `AWS_PROFILE` |

## Examples

### Full workflow: create repository, build, and push

```bash
# Set environment variables
export AWS_ACCOUNT_ID=123456789012
export AWS_REGION=us-west-2

# Create the ECR repository (idempotent)
aux4 registry aws create myapp

# Build and push
docker build -t myapp:v1.0.0 .
aux4 registry aws push myapp:v1.0.0 --repository myapp --imageTag v1.0.0
```

### Push with explicit flags (no environment variables)

```bash
aux4 registry aws push myapp:latest \
  --repository myapp \
  --imageTag latest \
  --region us-east-1 \
  --accountId 123456789012
```

## License

Apache-2.0. See [LICENSE](./LICENSE) for details.
