# Flux Kubernetes Configuration Repository

This repository contains the GitOps configuration for managing Kubernetes resources using Flux. It follows the [GitOps](https://www.gitops.tech/) principles for managing infrastructure and application deployments.

## Repository Structure

```plaintext
.
├── flux-system/             # Flux system components and configuration
│   ├── gotk-components.yaml # Core Flux controllers and CRDs
│   ├── gotk-sync.yaml      # Flux GitRepository and Kustomization
│   └── kustomization.yaml  # Kustomize configuration for Flux components
└── releases/               # Application releases and configurations
    ├── namespaces/        # Namespace definitions
    └── playground/        # Playground environment configurations
        ├── helloworld-app/  # Example application
        └── another-app/     # Another application
```

## Prerequisites

- Kubernetes cluster
- Flux CLI installed
- kubectl configured with cluster access
- GitHub personal access token (for private repositories)

## Setup

1. Install Flux in your cluster:

```bash
flux bootstrap github \
  --owner=<your-github-username> \
  --repository=flux-k8s \
  --branch=main \
  --path=./ \
  --personal
```

2. Verify the installation:

```bash
flux get all
```

## Application Deployment

### Directory Structure

Applications are organized in the `releases` directory:

- `namespaces/`: Contains namespace definitions
- `playground/`: Contains application deployments for the playground environment
  - Each application has its own directory with Kubernetes manifests

### Adding a New Application

1. Create a new directory under the appropriate environment in `releases/`
2. Add your Kubernetes manifests
3. Include the application in the environment's `kustomization.yaml`

Example:
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
- new-app/deployment.yaml
```

## Features

- **Automated Synchronization**: Changes pushed to this repository are automatically synchronized with the cluster
- **Image Automation**: Automatic updates of container images using Flux image automation controllers
- **Multi-Environment Support**: Organized structure for managing multiple environments
- **Namespace Isolation**: Applications are deployed in their own namespaces for better isolation

## Image Updates

The repository supports automated image updates using Flux image automation controllers. Image policies are defined in the application manifests:

```yaml
apiVersion: image.toolkit.fluxcd.io/v1beta2
kind: ImagePolicy
metadata:
  name: example-app
  namespace: flux-system
spec:
  imageRepositoryRef:
    name: example-app
  policy:
    semver:
      range: '>=0.0.1'
```

## Monitoring

Flux provides various ways to monitor the synchronization status:

```bash
# Get all Flux resources
flux get all

# Check reconciliation status
flux get kustomizations --watch

# Check Git repository status
flux get sources git
```

## Troubleshooting

Common issues and solutions:

1. **Synchronization Issues**
   ```bash
   flux logs --level=error
   ```

2. **Resource Conflicts**
   ```bash
   kubectl get events --all-namespaces
   ```

3. **Image Update Issues**
   ```bash
   flux get images all
   ```

## Best Practices

1. **Commit Convention**: Use conventional commits for better clarity
   - feat: New feature
   - fix: Bug fix
   - docs: Documentation changes
   - chore: Maintenance tasks

2. **Resource Organization**
   - Group related resources in the same directory
   - Use meaningful names for resources and files
   - Keep environment-specific configurations separate

3. **Security**
   - Use sealed secrets for sensitive data
   - Implement RBAC policies
   - Regular security scanning of manifests

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.
```

This documentation follows industry best practices by including:
1. Clear project structure explanation
2. Setup instructions
3. Usage guidelines
4. Monitoring and troubleshooting sections
5. Best practices
6. Contributing guidelines

The format is clean and organized, making it easy for both new and experienced users to understand and use the repository. You can further customize it based on your specific needs or requirements.