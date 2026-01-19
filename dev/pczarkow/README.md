# pczarkow Cluster Configuration

This directory contains the GitOps configuration for the `pczarkow` ROSA HCP cluster.

## Files

- **`infrastructure.yaml`**: Infrastructure components (EFS, logging, compliance, etc.)
- **`applications-ns.yaml`**: Application teams and namespace configurations

## Configuration Updates Required

After Terraform creates the cluster and AWS resources, update the following values in `infrastructure.yaml`:

### Get Values from Terraform Outputs

1. **AWS Account ID**: Get from Terraform output
   ```bash
   terraform -chdir=../../../../terraform/infrastructure output -raw aws_account_id
   ```
   Or manually:
   ```bash
   aws sts get-caller-identity --query Account --output text
   ```

2. **EFS File System ID**: Get from Terraform output
   ```bash
   terraform -chdir=../../../../terraform/infrastructure output -raw efs_file_system_id
   ```

3. **IAM Role ARNs**: The role ARNs are automatically created by Terraform with the pattern:
   - `{cluster_name}-rosa-efs-csi-role-iam` (e.g., `pczarkow-rosa-efs-csi-role-iam`)
   - `{cluster_name}-rosa-cloudwatch-role-iam` (e.g., `pczarkow-rosa-cloudwatch-role-iam`)

   These should match the cluster name in your Terraform configuration.

### Update infrastructure.yaml

Replace the placeholders in `infrastructure.yaml`:
- `ACCOUNT_ID` → Your AWS account ID (from Terraform output)
- `fs-XXXXXXXXXXXXX` → EFS file system ID (from Terraform output)

## Git Path

When configuring the bootstrap script, use:
```bash
export GIT_PATH="dev/pczarkow"
```

## Adding Infrastructure Components

To add more infrastructure components, add entries to the `infrastructure` list in `infrastructure.yaml`. See the reference examples in `reference/pfoster/cluster-config/` for more component options.

## Adding Application Teams

To add application teams:
1. Create a namespace configuration file in `namespaces/{team}-{apmnum}.yaml`
2. Add the team entry to `applications-ns.yaml`
3. Commit and push to the Git repository
