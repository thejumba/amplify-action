# Amplify Github Action

This is a very simple action intended to help you deploy your amplify app to AWS. It can be used to push or pull the amplify app. The action requires a minimum number of inputs. Refer to the documentation below for more information.

## Example usage

```yaml
name: example-workflow

on:
  push:
    branches:
      - main

# make sure to add the necessary permissions to the workflow
permissions:
    id-token: write

# ...
jobs:
    pull-amplify:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout code
          uses: actions/checkout@v2

        - name: Pull amplify environment
          uses: thejumba/amplify-action@main
          with:
              action: pull
              amplify_env: staging
              aws_region: us-east-1
              amplify_app_id: ********
              role_arn: arn:aws:iam::******:role/role-name
```

The example shown above uses the minimum number of inputs required to pull the amplify app. The inputs are as follows:

- `action`: The action to be performed. This can either be `pull` or `push`.
- `amplify_env`: The name of the amplify environment to be pulled or pushed to.
- `aws_region`: The AWS region where the amplify app is located. Default is `us-east-1`.
- `amplify_app_id`: The amplify app id. This can be found in the amplify console or the `amplify/team-provider-info.json` file.
- `role_arn`: The role arn to be assumed when pulling or pushing the amplify app. This is required since this workflow uses the OIDC flow to obtain temporary credentials to access AWS resources. Learn more about different authorization flows [here](https://github.com/aws-actions/configure-aws-credentials)
- `skip_cli_install`: This is an optional input that can be used to skip the installation of the amplify CLI. Default is `false`.
- `amplify_cli_version`: This is an optional input that can be used to specify the version of the amplify CLI to be installed. Default is `latest`.
- `deploy_command`(optional): When pushing to an amplify environment, you may want to override/customize the command used to push. This is the input to use to specify the command. Default is `amplify push --yes --force --allow-destructive-graphql-schema-updates --minify`.
