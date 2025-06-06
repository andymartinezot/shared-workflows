# Shared GitHub Workflows

This repository contains reusable GitHub Actions workflows that can be used across multiple repositories to standardize common CI/CD processes.

## Docker Image Build Workflow

The `build-docker.yaml` workflow provides a standardized way to build and push Docker images to a container registry. This workflow can be reused across multiple repositories to maintain consistency in Docker image builds.

### Usage

To use this workflow in your repository, add the following to your workflow file:

```yaml
name: Build and Deploy

on:
  push:
    branches: [ main ]  # or your default branch
  # Add other triggers as needed

jobs:
  build:
    uses: andymartinezot/shared-workflows/.github/workflows/build-docker.yaml@main
    with:
      image_name: your-image-name
      dockerfile_path: ./Dockerfile  # optional, defaults to '.'
      context_path: .                # optional, defaults to '.'
      registry_username: your-registry-username
    secrets:
      REGISTRY_TOKEN: ${{ secrets.REGISTRY_TOKEN }}
```

### Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `image_name` | Yes | - | The name of the image to build (e.g., "my-app-image") |
| `dockerfile_path` | No | '.' | The path to the Dockerfile to build (e.g., ./Dockerfile or ./app/Dockerfile) |
| `context_path` | No | '.' | The path to the build context (Files to be copied to the image) |
| `registry_username` | Yes | - | The username of the registry to push the image to |

### Secrets

| Secret | Required | Description |
|--------|----------|-------------|
| `REGISTRY_TOKEN` | Yes | The token for authentication with the container registry |

### Outputs

| Output | Description |
|--------|-------------|
| `image-tag` | The full tag of the built image (format: registry_username/image_name:latest) |

### Image Tags

The workflow automatically tags the Docker image with:
- `latest`
- Date-based tag (YYYYMMDD format)
- Git commit SHA

For example, if your image name is "my-app", the following tags will be created:
- `amartinezot/my-app:latest`
- `amartinezot/my-app:20240321` (current date)
- `amartinezot/my-app:abc123` (git commit SHA)

### Requirements

- GitHub Actions enabled in your repository
- Access to a container registry
- Valid registry credentials (username and token)

### Example

Here's a complete example of how to use this workflow:

```yaml
name: Build and Deploy My App

on:
  push:
    branches: [ main ]

jobs:
  build-docker:
    uses: andymartinezot/shared-workflows/.github/workflows/build-docker.yaml@main
    with:
      image_name: my-app
      dockerfile_path: ./Dockerfile
      context_path: .
      registry_username: amartinezot
    secrets:
      REGISTRY_TOKEN: ${{ secrets.REGISTRY_TOKEN }}
```

## Contributing

Feel free to submit issues and enhancement requests!
