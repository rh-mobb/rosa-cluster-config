# `public-ocp5` — ROSA HCP public + OCP 5.x (+ layer-ocp-agentic)

Installer recipe: [validated-pattern-terraform-rosa `clusters/public-ocp5`](https://github.com/rh-mobb/validated-pattern-terraform-rosa/tree/main/clusters/public-ocp5).

- OCP **5.x** (`channel_group: fast`) with RHCS **1.7.14+**
- ESO + platform metadata (`enable_secrets_manager_iam` / GitOps bootstrap)
- Optional: pair with [layer-ocp-agentic](https://github.com/rhcs-validated-patterns/layer-ocp-agentic) (`ROSA_PROFILE=public-ocp5`)

Set `gitops_git_path = "dev/public-ocp5"` in installer tfvars.