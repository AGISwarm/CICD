# Installation

1. In docker-compose.yml, fill the following fields:

```yaml
services:
    self_hosted_github_actions:
        build:
        args:
            repo_url: <REPO_URL>
            runner_name: <RUNNER_NAME>  # Optional
secrets:
    github_actions_token:
        file: <PATH_TO_GITHUB_ACTIONS_TOKEN>

```

2. Run the following command:

```bash
docker-compose up -d
```