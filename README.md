# upload-to-onedrive

GitHub action to upload a single file to OneDrive application folder.

## Inputs

| Name                | Description                                              | Default                       |
| ------------------- | -------------------------------------------------------- | ----------------------------- |
| `api-endpoint`      | API endpoint for OneDrive API                            | https://api.onedrive.com/v1.0 |
| `client-id`         | Application (client) ID                                  | (Required)                    |
| `client-secret`     | Client secret                                            | (Required)                    |
| `conflict-behavior` | How to handle conflict ("rename", "fail" or "replace")   | `rename`                      |
| `path`              | Path of the file to be uploaded                          | (Required)                    |
| `refresh-token`     | Refresh token from the user consented to the application | (Required)                    |

## Outputs

| Name                | Description                                           | Example                                                              |
| ------------------- | ----------------------------------------------------- | -------------------------------------------------------------------- |
| `created-date-time` | File creation time in ISO timestamp                   | `"2025-11-29T11:24:00Z"`                                             |
| `ctag`              | ETag of the content of the uploaded file              | `"\"c:{12345678-1234-5678-ABCD-12345678ABCD},2\""`                   |
| `etag`              | ETag of the content and metadata of the uploaded file | `"\"{12345678-1234-5678-ABCD-12345678ABCD},3\""`                     |
| `item-id`           | Item ID of the uploaded file                          | `"0123456789abc!123"`                                                |
| `quick-xor-hash`    | Quick XOR hash of the uploaded file                   | `"a+base64+value=="`                                                 |
| `url`               | URL of the uploaded file                              | `"https://onedrive.live.com?cid=0123456789abc&id=0123456789abc!123"` |

## Step-by-step

### Obtain a refresh token

1. Create Entra ID application registration
   - Follow the [official instruction](https://learn.microsoft.com/en-us/graph/auth-register-app-v2)
   - Set the "Redirect URI" to https://compulim.github.io/upload-to-onedrive/redirect
2. Go to https://compulim.github.io/upload-to-onedrive/
   - Paste your application (client) ID
   - Click "Authorize"
3. Consent your own application
4. Then, it should redirect back to https://compulim.github.io/upload-to-onedrive/redirect
   - Paste your application (client) ID
   - Paste your client secret
   - Copy the cURL script and run it in your terminal
   - Obtain the `refresh_token`, this is your refresh token

### Set up GitHub workflow

Adds the following to your GitHub workflow.

```yaml
- uses: compulim/upload-to-onedrive@main
  with:
    client-id: ${{ vars.ONEDRIVE_CLIENT_ID }}
    client-secret: ${{ secrets.ONEDRIVE_CLIENT_SECRET }}
    path: ${{ steps.build-args.outputs.filename }}
    refresh-token: ${{ secrets.ONEDRIVE_REFRESH_TOKEN }}
```

Adds the following in your GitHub environment.

- Secrets
  - `ONEDRIVE_CLIENT_SECRET` with your client secret from Entra ID
  - `ONEDRIVE_REFRESH_TOKEN` with your refresh token
    - Should starts with to `M.C...`
- Variables
  - `ONEDRIVE_CLIENT_ID` with your application (client) ID from Entra ID

## Roadmap

- [ ] Release deployment
- [ ] Support glob for uploading multiple files
  - How should we return the output values?
  - [ ] Support directory hierarchy
- [ ] Support multi-tenant
- [ ] Automated testing against OneDrive Personal
- [ ] Automated testing against OneDrive for Business

## Contributions

Like us? [Star](https://github.com/compulim/upload-to-onedrive/stargazers) us.

Want to make it better? [File](https://github.com/compulim/upload-to-onedrive/issues) us an issue.

Don't like something you see? [Submit](https://github.com/compulim/upload-to-onedrive/pulls) a pull request.
