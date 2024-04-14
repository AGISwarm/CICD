# Installation
## Get a GitHub Actions Token
1. Go to your GitHub repository.
2. Click on `actions` tab.
3. Click on `runners` tab.
4. Click on `New self-hosted runner`.
5. Under section `Configure` you will find a token. Copy it.
   ```bash
   ./config.cmd --url <YOUR_REPO_URL> --token <YOUR_TOKEN_TO_COPY>
    ```
6. Save the token in a file.

## Setup docker-compose.yml

```yaml
services:
    self_hosted_github_actions:
        build:
        args:
            repo_url: <YOUR_REPO_URL>
            runner_name: <RUNNER_NAME>  # Optional
secrets:
    github_actions_token:
        file: <PATH_TO_GITHUB_ACTIONS_TOKEN>

```

## Run the following command:

```bash
docker-compose up -d
```