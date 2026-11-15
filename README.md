# Continuous Integration

Minimal Github Actions docker image.

```yaml
jobs:
  ci:
    runs-on: self-hosted
    container:
      image: docker.io/cloudfra/ci-core:canary
    permissions:
      packages: write
      contents: write
      id-token: write
    timeout-minutes: 45
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Container Fix
        run: git config --system --add safe.directory /__w/
```

## template-go update routine

This repository was not created from [template-go](https://github.com/cloudfra/template-go), but it
carries the template's update scripts so its CI configuration can be re-synced from inside the repo:

```sh
scripts/template-go/sync-template.sh -n   # preview, changes nothing
scripts/template-go/sync-template.sh      # apply on a branch, review, open a pull request
```

`sync-template.sh` hard-replaces `Makefile_*.mk`, the root dotfile configs, `.github/` and
`scripts/template-go/` with the upstream versions. Only `.github/workflows/deploy-*.yaml` survive a
`.github/` replacement, so as this repo stands the sync would also take
`.github/workflows/publish-ci-core.yaml` and `.github/workflows/reviewdog.yml` with it; rename them
to `deploy-*.yaml` first if they should stay. `update-project.sh` reads tool-version pins from the
template's `Makefile_*.mk` includes and refreshes Go modules through `make deps`, neither of which
this repo has.

## Links

* [GitHub](https://github.com/cloudfra/ci)
* [Docker Hub](https://hub.docker.com/repository/docker/cloudfra/ci-core/)
