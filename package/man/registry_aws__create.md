#### Description

The `create` command creates a new ECR repository with the given name. If the repository already exists, the command succeeds without making any changes. This makes it safe to call repeatedly (idempotent).

Internally, it first checks if the repository exists by running `aws ecr describe-repositories`. If that check fails (repository not found), it runs `aws ecr create-repository` to create it. The repository name is a positional argument.

#### Usage

```bash
aux4 registry aws create <repository> [--region <region>] [--profile <profile>]
```

repository  The ECR repository name (positional, required)
--region    AWS region (default: us-east-1, env: AWS_REGION)
--profile   AWS CLI profile (env: AWS_PROFILE)

#### Example

```bash
aux4 registry aws create myapp --region us-west-2
```

```json
{
  "repository": {
    "repositoryArn": "arn:aws:ecr:us-west-2:123456789012:repository/myapp",
    "registryId": "123456789012",
    "repositoryName": "myapp",
    "repositoryUri": "123456789012.dkr.ecr.us-west-2.amazonaws.com/myapp"
  }
}
```
