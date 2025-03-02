# google-docs-to-github

Custom action to export Google Docs to GitHub.

This action searches docs in the specified folder on Google Drive,
convert them into Markdown files, then push them to the GitHub repository.

## Set up

We recommend that you create a new service account on Google Cloud for this action, share the Google Drive folder with this account,
and authenticate to Google Cloud by using [google-github-actions/auth](https://github.com/google-github-actions/auth),
that automatically set up `GOOGLE_APPLICATION_CREDENTIALS` environment variable to access Google Drive API.

This action requires at least one of the following authorization scopes:

- `https://www.googleapis.com/auth/drive`
- `https://www.googleapis.com/auth/drive.file`
- `https://www.googleapis.com/auth/drive.readonly`

## Usage

This is an example workflow to export docs daily at 00:00 (GMT).

```yaml
# .github/workflows/import.yml
name: import

on:
  schedule:
    - cron: "0 0 * * *"

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: google-github-actions/auth@v0
        with:
          service_account: my-service-account-id@my-project-id.iam.gserviceaccount.com
          workload_identity_provider: projects/my-project-id/locations/global/workloadIdentityPools/my-pool-id/providers/my-provider-id
      - uses: r7kamura/google-docs-to-github@v3
        with:
          google_drive_folder_id: 1zD5A9LcT1aHz5_R_eXvikWy1l7SGcjH_
```

## Inputs

### `github_branch`

GitHub branch name where files will be pushed.

- optional
- default: `"main"`

### `github_directory`

GitHub directory path where files will be pushed.

- optional
- default: `"google_docs"`

### `github_token`

GitHub access token to push files.
If you want to run other workflows automatically by pusing files, you need to pass a personal access token with `workflow` permission.

- optional
- default: Automatically generated token is used via `github.token`.

### `google_drive_folder_id`

ID of the Google Drive folder to be exported. Folder ID is contained in the trailing part of the folder's URL.

- required
- example: `"1zD5A9LcT1aHz5_R_eXvikWy1l7SGcjH_"`
- 
### `recursive`

Export files from subdirectories and keep folder structure

## Related projects

- https://github.com/r7kamura/google-docs-to-markdown
- https://github.com/r7kamura/google-docs-to-local
