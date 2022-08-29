# Delete Branch Cache Action

[![CI](https://github.com/snnaplab/delete-branch-cache-action/actions/workflows/ci.yml/badge.svg)](https://github.com/snnaplab/delete-branch-cache-action/actions/workflows/ci.yml)

Deletes GitHub Actions caches for a branch. 

Besides if you want to clear the caches for some reason, you can save cache space by deleting the caches you no longer need immediately, because [total size of all caches in a repository is limited to 10 GB](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#usage-limits-and-eviction-policy).

## Inputs

| Name | Description | Default | Required |
| --- | --- | --- | --- |
| `repository` | Target repository name with owner, eg `snnaplab/delete-branch-cache-action`. Specify when deleting the cache of a repository different from the workflow. | `${{ github.repository }}` | false |
| `ref` | Git reference of target branch in `refs/heads/<branch name>` format. To reference a pull request use `refs/pull/<number>/merge` format. | `${{ github.ref }}` | false |
| `key` | Delete caches with keys that prefix match the key specified here. If not specified, all caches of the branch specified by `ref` will be deleted. || false |
| `github-token` | Specify a personal access token (PAT) when targeting a repository different from the workflow. | `${{ github.token }}` | false |

## Example

### Remove unused caches when closing a pull request

Caches created in the HEAD branch of a pull request are no longer used, so delete them immediately to save repository cache space.

```yaml
name: Delete Unused Caches

on:
  pull_request:
    types: [closed]

jobs:
  delete:
    runs-on: ubuntu-latest
    steps:
      - uses: snnaplab/delete-branch-cache-action@v1
        with:
          # Since the ref at the time of merging will be `main` or `develop`, specify it explicitly
          ref: refs/pull/${{ github.event.number }}/merge
```

## License

[MIT](LICENSE)
