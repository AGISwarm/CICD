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

### Authentication Configuration

1. Create a `.htpasswd` file for the Docker Registry.
   1. Run this command and follow the instructions to create a password
    ```bash
    htpasswd -c .htpasswd <USERNAME>
    ```
   2. Move the `.htpasswd` file to the `docker-registry` directory.
    ```bash
    mv .htpasswd docker-registry/
    ```

2. Create a `.htpasswd` file for the PyPI server.
   1. Run this command and follow the instructions to create a password
    ```bash
    htpasswd -c .htpasswd <USERNAME>
    ```
   2. Move the `.htpasswd` file to the `pypiserver` directory.
    ```bash
    mv .htpasswd pypi-server/
    ```

## Starting Services

To launch all services:
```bash
docker-compose up -d
```

Now, you have a self-hosted CI/CD environment with a GitHub Actions runner, a private Docker registry, and a PyPI server.
Refer to [python-template](https://github.com/AGISwarm/python-template) for an example repository that uses this CI/CD environment.

## Using the PyPI Server and the Docker Registry directly from your host machine

With Nginx running, you can access the PyPI server and the Docker Registry from your host machine by their service names, without needing to expose their ports.
For docker registry, you have to specify default port 80. Otherwise docker commands will not work.
You do not need to manage ports or IP addresses manually.

### PyPI Server

To upload a package to the PyPI server:
```bash
twine upload --repository-url http://pypi-server/ dist/* --username <USERNAME> --password <PASSWORD>
```

To install a package from the PyPI server:
```bash
pip install --extra-index-url http://pypi-server/ --trusted-host pypi-server <PACKAGE_NAME>
```

### Docker Registry

To push an image to the Docker Registry:
```bash
docker tag <IMAGE_NAME> docker-registry:80/<IMAGE_NAME>
docker login docker-registry:80 -u <USERNAME> -p <PASSWORD>
docker push docker-registry:80/<IMAGE_NAME>
```

To pull an image from the Docker Registry:
```bash
docker login docker-registry:80 -u <USERNAME> -p <PASSWORD>
docker pull docker-registry:80/<IMAGE_NAME>
```

## Stopping Services

To stop all services:
```bash
docker-compose down
```

## License

This project is licensed under the MIT License - see the `LICENSE.txt` file for details.