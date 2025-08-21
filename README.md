[![DelivOps banner](https://raw.githubusercontent.com/delivops/.github/main/images/banner.png?raw=true)](https://delivops.com)

# sam-deploy-action

This repository provides a reusable GitHub Action for deploying applications to AWS using the AWS SAM (Serverless Application Model) framework. It automates the deployment process, allowing you to deploy ECS services using SAM with customizable options for dry-run and full deployment.

By using this action, you can streamline your deployment pipeline, ensuring consistent and repeatable deployments to AWS.

## How it works

This GitHub Action follows these steps to deploy ECS services using AWS SAM:

1. **Checkout Repository**: It checks out your GitHub repository to the workflow runner.
2. **Configure AWS Credentials**: It configures AWS credentials using IAM role assumption to interact with AWS services securely.
3. **Deploy ECS Service**:
    - **Dry-Run Deployment**: If the `dry_run` input is true, it simulates the deployment without applying any changes.
    - **Full Deployment**: If the `dry_run` input is false, it applies the changes to AWS.
4. **Print Deployment Outputs**: After a successful deployment, it retrieves and prints the outputs of the CloudFormation stack.

## Features

✔️ **Full & Dry-Run Deployment**: Supports both actual and simulated deployments to AWS SAM.

✔️ **Customizable Deployment Parameters**: Easily customize the deployment environment, stack name, and SAM config path.

✔️ **Secure AWS Authentication**: Uses IAM role assumption for secure and controlled AWS access.

✔️ **Reusable Workflow**: Can be called from other workflows with different inputs for flexibility.

## Inputs

- `dry_run`: A boolean flag to perform a dry-run deployment (default: `false`).
- `aws_region`: AWS region to use for deployment (required).
- `aws_account_id`: AWS account id (required).
- `aws_role`: AWS role (required).
- `dockerfile_path`: Path to the Dockerfile (required).
- `sam_config_path`: Path to the SAM config file (required).
- `stack_name`: The CloudFormation stack name for the deployment (required).
- `sam_template_path`: Path to the SAM template file (required).

## Example Usage

```yaml
name: SAM Deploy

on:
  workflow_dispatch:

jobs:
  deploy:
    uses: delivops/sam-deploy-action@main
    with:
      aws_region: us-east-1
      aws_account_id: ${{secrets.AWS_ACCOUNT_ID}}
      aws_role: github-services
      sam_config_path: globals/samconfig.yaml
      stack_name: app244
      sam_template_path: services/app2/simple.yaml
```

## Behavior Scenarios

✅ **Dry-Run Deployment**  
Simulates the deployment without making any changes to the infrastructure.  
No changes are applied to AWS, and no outputs are printed.

⚠️ **Full Deployment**  
Applies the deployment to AWS using SAM.  
The ECS service is updated, and CloudFormation stack outputs are printed.

❌ **Workflow Failure**  
If the deployment fails due to any errors (e.g., invalid configuration, AWS credentials issues), an error message will be displayed.  
If configured, the failure can trigger a notification to the team via Slack or GitHub Issues.
