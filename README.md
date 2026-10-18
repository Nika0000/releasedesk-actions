# ReleaseDesk Publish

Upload, promote, distribute and revoke releases on a self-hosted [ReleaseDesk](https://releasedesk.dev) instance. Wraps the `rdesk` CLI: the action looks up the latest stable build in the public `rdesk-cli` update feed on releasedesk.dev, verifies its SHA-256 and caches it.

## Usage

Upload on tag, then promote:

```yaml
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - id: upload
        uses: Nika0000/releasedesk-action@v1
        with:
          command: upload
          server-url: ${{ vars.RD_URL }}
          token: ${{ secrets.RD_TOKEN }}
          file: build/app.zip
          version: ${{ github.ref_name }}
          platform: linux
          arch: x64
          format: zip
          notes-file: RELEASE_NOTES.md
      - uses: Nika0000/releasedesk-action@v1
        with:
          command: promote
          server-url: ${{ vars.RD_URL }}
          token: ${{ secrets.RD_TOKEN }}
          release-id: ${{ steps.upload.outputs.release-id }}
          channel: beta
          percent: 10
```

## Inputs

| Input | Used by | Notes |
| --- | --- | --- |
| `command` | all | `upload`, `asset`, `promote`, `distribute`, `revoke` |
| `server-url`, `token` | all | required; use a CI-scoped API token stored as a secret |
| `org`, `app` | all | default to the token's own scope |
| `file`, `version`, `build`, `platform`, `arch`, `format`, `notes-file` | upload | `build` defaults to the run number |
| `release-id`, `role`, `file` | asset | |
| `release-id`, `channel`, `percent` | promote | `percent` defaults to 100 |
| `release-id`, `groups` | distribute | comma- or newline-separated |
| `release-id` | revoke | |
| `rdesk-feed` | all | feed the CLI is downloaded from; defaults to the ReleaseDesk `rdesk-cli` stable feed on releasedesk.dev |

## Outputs

`release-id`, `version`, `artifact-sha256` (upload) and `json` (raw CLI result).

## Supported runners

Linux x64/arm64, macOS x64/arm64, Windows x64. Requires `bash`, `curl`, `jq` and `unzip` (preinstalled on GitHub-hosted runners).
