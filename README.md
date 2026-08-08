# Dagny's Kargo Quickstart

## Proposed Enhancement: All Systems (K)argo

I would add **All Systems (K)argo**, a lightweight setup-readiness and verification utility for the Kargo quickstart.

The quickstart currently surfaces many prerequisite, credential, and compatibility issues only after setup has begun. The utility would identify locally detectable problems earlier so technically capable users who are new to Akuity can focus on learning Kargo and Argo CD rather than troubleshooting their environment.

### Scope

Before provisioning, the utility would verify that:

- Required CLIs are installed.
- Required environment variables are populated.
- A k3d cluster exists and `kubectl` can reach it.
- GitHub, GHCR, and Kargo repository references are lowercase and consistent.
- Docker can access the expected GHCR image without exposing the PAT.
- `akuity/argocd.yaml` contains a nonempty `spec.version`.

After provisioning, it would use available CLI and Kubernetes APIs to confirm that the expected Kargo resources, Argo CD Applications, and agent connections can be queried.

Blocking issues would produce actionable failure messages, while non-blocking concerns would appear as warnings.

### Assumptions

- The primary user would understand Kubernetes, GitOps, and command-line tools but may be new to Kargo and the Akuity Platform.
- The user would run the official quickstart template in its Linux-based GitHub Codespace with the provided k3d cluster.
- The user would already have the required GitHub and Akuity accounts and permissions; the utility would validate access but would not create accounts or credentials.
- The quickstart’s expected file structure and resource names would remain unchanged.
- Platform-controlled conditions, such as instance quotas and supported versions, may not be available for preflight validation.

### Design Choices

I would keep the utility separate from the existing provisioning scripts, safe to rerun, and non-destructive. It would inspect configuration and connectivity without creating, modifying, deleting, or automatically repairing resources.

I would also avoid printing secrets, reporting only whether required credentials are present and usable. This focused scope would make the utility realistic to build and maintain while addressing the most common locally detectable failures.

The utility would complement—not replace—the written documentation. Fast-changing technical prerequisites could be expressed as executable checks, allowing the guide to remain focused on explaining the Kargo workflow.

### Tradeoffs and Limitations

The script would require maintenance as the quickstart and platform evolve. It also could not reliably predict server-side conditions such as instance quotas or dynamically determine every supported Argo CD version.

For conditions that cannot be validated in advance, it would preserve the platform error and provide a clearer explanation and suggested next step. The goal would be to catch preventable problems earlier and make remaining failures easier to understand.

### Usage

```bash
bash scripts/all-systems-kargo.sh
```

A successful check would report:

```text
All systems (K)argo — ready to proceed.
```
