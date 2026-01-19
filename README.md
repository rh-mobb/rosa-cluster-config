# Cluster Config GitOps Repository Template

This is a template GitOps repository structure for managing ROSA HCP cluster configurations using ArgoCD and the App-of-Apps pattern.

## Repository Structure

```
cluster-config/
├── README.md                    # This file
├── .gitignore                  # Git ignore rules
├── {environment}/              # Environment directory (e.g., dev, staging, prod, nonprod)
│   └── {cluster-name}/         # Cluster-specific directory
│       ├── infrastructure.yaml  # Infrastructure components configuration
│       ├── applications-ns.yaml # Application namespace configuration
│       └── namespaces/          # Optional: Team/application namespace configs
│           └── {team}-{apmnum}.yaml
└── templates/                  # Optional: Template files for new clusters
    ├── infrastructure.yaml.template
    └── applications-ns.yaml.template
```

## Usage

### 1. Initialize as Git Repository

```bash
cd reference/cluster-config
git init
git add .
git commit -m "Initial commit: Cluster config template"
git remote add origin https://github.com/your-org/cluster-config.git
git push -u origin main
```

### 2. Create Cluster Configuration

For each cluster, create a directory structure:

```bash
mkdir -p {environment}/{cluster-name}
cp templates/infrastructure.yaml.template {environment}/{cluster-name}/infrastructure.yaml
cp templates/applications-ns.yaml.template {environment}/{cluster-name}/applications-ns.yaml
```

### 3. Configure Cluster-Specific Values

Edit the YAML files with cluster-specific values:
- Update `AWS_REGION` in `infrastructure.yaml`
- Update `clusterName` in both files
- Add infrastructure components as needed
- Add application teams as needed

### 4. Git Path Format

The `gitPath` used in Terraform/bootstrap script should match the directory structure:
- Directory: `nonprod/np-hub/`
- gitPath: `nonprod/np-hub`

## Configuration Files

### infrastructure.yaml

Defines infrastructure components deployed via GitOps:
- Storage (EFS, EBS)
- Logging (CloudWatch)
- Compliance operators
- Security operators
- Platform tools

### applications-ns.yaml

Defines application teams and their namespaces:
- Team configurations
- ArgoCD project setup
- Namespace definitions

### namespaces/{team}-{apmnum}.yaml

Team-specific namespace configurations:
- Namespace definitions
- Resource quotas
- RBAC configurations
- Service accounts

## Integration with Terraform

This repository is referenced by the Terraform bootstrap script via the `GIT_PATH` environment variable:

```bash
export GIT_PATH="nonprod/np-hub"
```

The bootstrap script will:
1. Install the `cluster-bootstrap` Helm chart
2. Create ArgoCD ApplicationSets that reference this repository
3. ArgoCD will sync applications based on the YAML files in this repo

## Best Practices

1. **Version Control**: Keep all cluster configurations in Git
2. **Environment Separation**: Use separate directories for different environments
3. **Cluster Isolation**: Each cluster has its own directory
4. **Incremental Updates**: Start with empty infrastructure/applications lists and add components gradually
5. **Documentation**: Document any custom configurations or decisions

## References

- [App-of-Apps Infrastructure Chart](../../pfoster/helm-repository/charts/app-of-apps-infrastructure/README.md)
- [App-of-Apps Application Chart](../../pfoster/helm-repository/charts/app-of-apps-application/README.md)
- [Cluster Bootstrap Chart](../../pfoster/helm-repository/charts/cluster-bootstrap/README.md)
