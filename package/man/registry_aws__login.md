#### Description

The `login` command authenticates your local Docker client with AWS Elastic Container Registry (ECR). It uses the AWS CLI to obtain a temporary authentication token via `aws ecr get-login-password` and passes it to `docker login` with the `AWS` username. The login is valid for the ECR registry endpoint corresponding to the given account ID and region.

You must have valid AWS credentials configured (via `aws configure`, environment variables, or an IAM role) before running this command. The `--accountId` flag is required unless the `AWS_ACCOUNT_ID` environment variable is set. The `--region` flag defaults to `us-east-1` and can also be set via `AWS_REGION`.

#### Usage

```bash
aux4 registry aws login --accountId <account-id> [--region <region>] [--profile <profile>]
```

--accountId  AWS account ID (required, env: AWS_ACCOUNT_ID)
--region     AWS region (default: us-east-1, env: AWS_REGION)
--profile    AWS CLI profile (env: AWS_PROFILE)

#### Example

```bash
aux4 registry aws login --accountId 123456789012 --region us-west-2
```

```text
Login Succeeded
```
