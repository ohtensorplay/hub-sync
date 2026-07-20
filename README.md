<p align="center">
  <a href="https://mega.tensorplay.cn/">
    <img src="https://mega.tensorplay.cn/assets/logo-D1t6EjrA.webp" alt="MEGA" width="420" />
  </a>
</p>

<p align="center"><strong>Keep models, datasets, and Spaces synchronized with MEGA Hub.</strong></p>

<p align="center">
  <a href="https://github.com/ohtensorplay/hub-sync/releases"><img alt="GitHub Release" src="https://img.shields.io/github/v/release/ohtensorplay/hub-sync?display_name=tag&sort=semver"></a>
  <a href="https://github.com/ohtensorplay/hub-sync/actions/workflows/live-smoke.yml"><img alt="Live smoke test" src="https://img.shields.io/github/actions/workflow/status/ohtensorplay/hub-sync/live-smoke.yml?label=live%20smoke"></a>
  <a href="https://github.com/ohtensorplay/hub-sync/blob/main/LICENSE"><img alt="MIT License" src="https://img.shields.io/github/license/ohtensorplay/hub-sync"></a>
</p>

# MEGA Hub Sync

Mirror a GitHub repository—or just one folder—into a MEGA Hub model, dataset,
or Space. The action creates the destination when needed, uploads changed
content, removes files that disappeared from the source, and adds a direct
repository link to the workflow summary.

## Quick start

Save a MEGA access token with repository write permission as the GitHub Actions
secret `MEGA_TOKEN`, then add this workflow:

```yaml
name: Sync to MEGA Hub

on:
  push:
    branches: [main]

permissions:
  contents: read

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7

      - uses: ohtensorplay/hub-sync@v1
        with:
          mega_repo_id: your-name/your-model
          mega_token: ${{ secrets.MEGA_TOKEN }}
          repo_type: model
```

Every push to `main` now updates `your-name/your-model` on
[MEGA Hub](https://mega.tensorplay.cn/).

## Common recipes

### Publish one directory

```yaml
- uses: ohtensorplay/hub-sync@v1
  with:
    mega_repo_id: your-organization/product-dataset
    mega_token: ${{ secrets.MEGA_TOKEN }}
    repo_type: dataset
    subdirectory: exports/latest
```

### Select files with globs

```yaml
- uses: ohtensorplay/hub-sync@v1
  with:
    mega_repo_id: your-name/production-model
    mega_token: ${{ secrets.MEGA_TOKEN }}
    repo_type: model
    include: |
      *.json
      *.safetensors
      tokenizer/**
    exclude: |
      **/*.tmp
      **/.DS_Store
```

### Keep destination-only files

Mirroring is enabled by default. Set `sync: "false"` when the action should
upload selected files without removing other content already on MEGA Hub.

```yaml
- uses: ohtensorplay/hub-sync@v1
  with:
    mega_repo_id: your-name/shared-assets
    mega_token: ${{ secrets.MEGA_TOKEN }}
    repo_type: dataset
    sync: "false"
```

## Inputs

| Input | Required | Default | Description |
| --- | :---: | --- | --- |
| `mega_repo_id` | Yes | — | Destination in `namespace/name` form. |
| `mega_token` | Yes | — | MEGA token with repository write permission. |
| `repo_type` | No | `space` | `model`, `dataset`, or `space`. |
| `subdirectory` | No | `.` | Folder inside the checked-out GitHub workspace. |
| `revision` | No | `main` | Destination branch. |
| `private` | No | `false` | Create a private destination when it does not exist. |
| `sync` | No | `true` | Remove destination files absent from the selected source tree. |
| `include` | No | — | Newline-separated globs to include. |
| `exclude` | No | `.git/**`, `.github/**` | Newline-separated globs to exclude. |
| `allow_empty` | No | `false` | Permit an intentionally empty selection. |
| `max_workers` | No | automatic | Maximum concurrent uploads. |
| `commit_message` | No | generated | Commit message recorded on MEGA Hub. |
| `github_repo_id` | No | current repository | Source name used in the generated commit message. |
| `mega_endpoint` | No | `https://mega.tensorplay.cn` | MEGA Hub endpoint. |
| `mega_version` | No | latest | Pin the `megatensors` package used by the action. |

## Outputs

| Output | Description |
| --- | --- |
| `repo_url` | URL of the synchronized MEGA repository. |
| `source_directory` | Canonical source directory used by the action. |

```yaml
- id: mega
  uses: ohtensorplay/hub-sync@v1
  with:
    mega_repo_id: your-name/your-model
    mega_token: ${{ secrets.MEGA_TOKEN }}
    repo_type: model

- run: echo "Published to ${{ steps.mega.outputs.repo_url }}"
```

## Security and versioning

- Store credentials in GitHub Actions secrets and grant only repository write
  access required by the destination.
- Source paths are restricted to the checked-out workspace; symlinks cannot
  escape the selected directory.
- Use `ohtensorplay/hub-sync@v1` for compatible v1 updates, or pin a full
  release such as `ohtensorplay/hub-sync@v1.0.0` for immutable workflows.

## License

[MIT](LICENSE) © MEGA
