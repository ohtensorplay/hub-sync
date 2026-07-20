<p align="center"><a href="https://mega.tensorplay.cn/"><img src="https://mega.tensorplay.cn/assets/logo-D1t6EjrA.webp" alt="MEGA" width="420" /></a></p>
<p align="center"><i>Synchronize model and dataset repositories from GitHub Actions.</i></p>
<p align="center">
  <img alt="GitHub Action" src="https://img.shields.io/badge/GitHub-Action-2088FF?logo=githubactions&logoColor=white">
  <a href="https://github.com/ohtensorplay/hub-sync/blob/main/LICENSE"><img alt="MIT License" src="https://img.shields.io/github/license/ohtensorplay/hub-sync"></a>
</p>

# MEGA Hub Sync Action

GitHub Action integration for synchronizing MEGA Hub content in CI pipelines.

## Use

Reference a published commit or release tag from a workflow, then provide only
the least-privileged MEGA token required by the operation. Keep credentials in
GitHub Actions secrets; never commit them to this repository or workflow files.

Use it to keep model, dataset, and repository automation reproducible across
your CI pipelines while MEGA handles the authenticated platform operations.

## Development

`action.yml` is the public Action contract. Validate metadata changes before
publishing and pin the version in consumer workflows.
