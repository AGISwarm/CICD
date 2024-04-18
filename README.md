# CICD Project

This project provides a self-hosted CI/CD environment using Docker, integrating a GitHub Actions runner, a private Docker registry, and a PyPI server.

## Components

- **Self-hosted GitHub Actions Runner**: Automated operations within your repository.
- **Docker Registry**: Private registry for Docker images.
- **PyPI Server**: Hosts Python packages internally.
- **Nginx**: Routes traffic to Docker Registry and PyPI server.

## Setup

### GitHub Actions Runner

Retrieve the token:
1. Go to your GitHub repository settings.
2. Navigate to `Actions` > `Runners` > `New self-hosted runner`.
3. Copy the token displayed during setup.

### Docker Configuration

Adjust the `docker-compose.yml` to fit your repository details and runner configuration:
```yaml
services:
  self_hosted_github_actions:
    build:
      args:
        repo_url: <YOUR_REPO_URL>
        runner_name: <OPTIONAL_RUNNER_NAME>
secrets:
    github_actions_token:
        file: <PATH_TO_GITHUB_ACTIONS_TOKEN>
```

### Starting Services

To launch all services:
```bash
docker-compose up -d
```

## Usage

This setup allows continuous integration and deployment within a self-managed environment, reducing external dependencies.

## License

This project is licensed under the MIT License - see the `LICENSE.txt` file for details.