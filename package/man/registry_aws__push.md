#### Description

The `push` command tags a local Docker image with the full ECR registry URI and pushes it to the specified ECR repository. Before pushing, it automatically runs `registry aws login` to authenticate with ECR using the same region and account ID.

The command performs three steps:

1. **Login** -- authenticates with ECR using `aws ecr get-login-password`
2. **Tag** -- tags the local image as `<accountId>.dkr.ecr.<region>.amazonaws.com/<repository>:<imageTag>`
3. **Push** -- pushes the tagged image to ECR

The `image` argument is positional and refers to the local Docker image (e.g., `myapp:v1.0.0`). The `--repository` flag specifies the ECR repository name. If `--imageTag` is not provided, it defaults to `latest`.

#### Usage

```bash
aux4 registry aws push <image> --repository <name> [--imageTag <tag>] [--region <region>] --accountId <account-id>
```

image        The local Docker image to push (positional, required)
--repository The ECR repository name (required)
--imageTag   The image tag (default: latest)
--region     AWS region (default: us-east-1, env: AWS_REGION)
--accountId  AWS account ID (required, env: AWS_ACCOUNT_ID)

#### Example

```bash
aux4 registry aws push myapp:v1.0.0 --repository myapp --imageTag v1.0.0 --accountId 123456789012
```

```text
Login Succeeded
The push refers to repository [123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp]
v1.0.0: digest: sha256:abc123... size: 1234
```
