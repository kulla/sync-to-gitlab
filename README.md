# Sync to Gitlab

This is a GitHub Action which automatically syncs a GitHub repository to a GitLab repository via a GitHub Action.

## Setup

1. Given a GitHub repository, create a new GitLab repository. For this action to work, the GitLab repository must not be empty (the default branch need to be created). The easiest way to archieve it is to import your GitHub repository into GitLab. Go to https://gitlab.com/projects/new and click "Import project". Select "GitHub" and follow the instructions.
2. Create an access token in GitLab. This can be done in the project settings under "Access Tokens". Make sure to select the `write_repository` scope and have the role `Developer`.
3. In case you sync a protected branch you need to make sure that the `Developer` role can push to it and that force pushes are allowed. This can be done in the project settings under "Repository" -> "Protected Branches". Select the branch you want to sync and make sure that the `Developer` role can push to it. Also make sure that the `Allow force push` checkbox is checked.
4. Add the created GitLab access token as a secret in your GitHub repository. Go to the settings of your GitHub repository, select "Secrets and variables" -> "Actions" and click "New repository secret". Name the secret `GITLAB_TOKEN` and paste the access token into the value field.

Now you can use the action in your GitHub repository. Create a file `.github/workflows/sync-to-gitlab.yml` and add the following content to it:

```yaml
name: Sync to GitLab
on:
  push:
    branches:
      # List all branches you want to sync to GitLab here
      - main

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Sync to GitLab
        uses: kulla/sync-to-gitlab@v1
        with:
          token: ${{ secrets.GITLAB_TOKEN }}
          # The following can be omitted if the GitLab owner is the same as the GitHub owner
          owner: <gitlab-username>
          # The following can be omitted if the GitLab repo is the same as the GitHub repo
          repository_name: <gitlab-repo>
```

See [`action.yml`](./action.yml) for all possible available inputs.
