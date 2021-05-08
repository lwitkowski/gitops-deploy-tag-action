# GitHub Action to update image tag in target Helm values.yaml

This [GitHub Action](https://github.com/actions) checks out target repo, updates image tag in specified target file and pushes the changes.

You can add this action to your GitHub workflow for Ubuntu runners (e.g. `runs-on: ubuntu-latest`) as follows:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    env:
      DEPLOY_GITOPS_REPO: 'lwitkowski/playground-gitops'
      DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}

    steps:
      - name: Deploy to test
        uses: lwitkowski/gitops-deploy-tag-action@master
        with:
          target-file: 'sample-service/test/values.yaml'

      - name: Deploy to production
        uses: lwitkowski/gitops-deploy-tag-action@master
        with:
          target-file: 'sample-service/prod/values.yaml'
```

TODO: how to setup `DEPLOY_TOKEN` secret? machine user token with repo write permissions

## Configuration

The action can be configured by the following options.

|Option|Default Value|Description|
|:-----|:-----:|:----------|
|`gitops-repo`|`${DEPLOY_GITOPS_REPO}`|Owner and name of target gitops repository name where yaml file is stored. |
|`target-file`|mandatory|Path to yaml file inside gitops repo to be updated.|
|`docker-image-tag`|`${GITHUB_SHA::6}`|Docker image tag to be set in target yaml file.|
|`env.DEPLOY_TOKEN`|mandatory|Github access token with write permission to gitops repo. Should be set as env variable in calling workflow.|

Note: Running this action on `pull_request_target` events is [dangerous if combined with code checkout and code execution](https://securitylab.github.com/research/github-actions-preventing-pwn-requests).