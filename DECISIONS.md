# Design Decisions

## Choices Made Based on Requirements

### 1. Python venv Location
- **Decision**: Created Python venv at `/opt/venv`
- **Rationale**: The `/opt` directory is a standard location for optional/add-on software packages in Linux. This makes it accessible to all users and follows FHS (Filesystem Hierarchy Standard).

### 2. Rust Installation Method
- **Decision**: Using Ubuntu's package manager (apt) to install Rust (rustc and cargo packages)
- **Rationale**: While rustup would provide the latest Rust version, the build environment has SSL certificate issues that prevent rustup from downloading. Using apt-installed Rust ensures a working installation. The version in Ubuntu 24.04's repositories (Rust 1.75+) is recent enough for most use cases. If the absolute latest Rust is required, rustup can be used when building in an environment with proper SSL certificates.

### 3. GitHub Actions Workflow Trigger
- **Decision**: Workflow triggers on pull_request events for all branches
- **Rationale**: The requirement specified "all pull requests" so using the `pull_request` trigger covers this comprehensively.

### 4. Docker Image Registry
- **Decision**: Using GitHub Container Registry (ghcr.io)
- **Rationale**: Native integration with GitHub Actions, no external credentials needed for public repos.

### 5. Docker Image Naming
- **Decision**: Image name follows pattern `ghcr.io/<owner>/<repo>/tt-smi-docker:latest`
- **Rationale**: Standard convention for GHCR, allows for clear identification and versioning. Using `tt-smi-docker` to distinguish the Docker image from other tt-smi related artifacts.

### 6. Base Image Tag
- **Decision**: Using `ubuntu:24.04` as the base image
- **Rationale**: Specified in requirements; using the official Ubuntu image from Docker Hub.

### 7. Container User
- **Decision**: Running container as root (default)
- **Rationale**: Simplifies access to `/opt/venv`. In production, a non-root user would be recommended, but not specified in requirements.

### 8. CMD Instruction
- **Decision**: Using `CMD ["ls", "-la", "/opt"]`
- **Rationale**: Requirement specified "final CMD should be an ls". Added `/opt` path to show the venv location and `-la` for detailed output.

### 9. Python Version
- **Decision**: Using Python 3.12 (default in Ubuntu 24.04)
- **Rationale**: Latest Python available in Ubuntu 24.04, no specific version requirement provided.

### 10. Workflow Permissions
- **Decision**: Added `packages: write` permission for GHCR push
- **Rationale**: Required for GitHub Actions to push to GitHub Container Registry.
