# Merge staging to main

Source script: [merge-staging-to-main.yml](../.github/workflows/merge-staging-to-main.yml)

## Description

This script exists to simplify the sync between branches, specifically `staging` and `main`.

### Inputs & Secrets

- reviewers: The list of reviewers to assign to the pull request. It is required and comma separated. Example: `reviewers: "user1, user2"`.
- Token: The token to use to authenticate to the GitHub API. It is required. Example: `token: "1234567890"`.

Note that the `token` secret needs to be a token with privileges to create a pull request, merge branches and trigger a workflow.

### Setup

If the `main` branch is protected with pull request rules it is important to allow that user to buypass these checks.
The settings of this can be found here:
`Settings -> branches -> [edit branch] -> "Allow specific actors to buypass pull request requirements"`

Type in the name of the user and save.

### Exceptions 

If the script fails to create a pull request it will still show up as "pass" in the CI with a message in the console.
The reason this is the case is that the script will trigger, even if there aren't any changes, which will cause the
`gh pr create` command to fail.

It is therefore important to verify the first run of this script for every repo that is going to use it, just to make sure
that it has the correct rights to create a pull request.

## Example usage

```yml
name: Merge staging into main
on:
  schedule:
    - cron: '0 8 * * 2,4'
jobs:
  merge-to-main:
    name: Merge staging into main
    uses: protectorinsurance/workflows/.github/workflows/merge-staging-to-main.yml@main
    with:
      reviewers: "myuser"
    secrets:
      token: ${{ secrets.MY_TOKEN }}
```