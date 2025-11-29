# upload-to-onedrive

GitHub action to upload a single file to OneDrive application folder.

## Step-by-step

### Obtaining consent and refresh token

1. Create Entra ID Application Registration
   - Follow the [official instruction](https://learn.microsoft.com/en-us/graph/auth-register-app-v2)
   - Redirect URI set to https://compulim.github.io/upload-to-onedrive/redirect
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

Add the following to your GitHub workflow

```yaml
- uses: compulim/upload-to-onedrive@main
  with:
    client-id: ${{ vars.ONEDRIVE_CLIENT_ID }}
    client-secret: ${{ secrets.ONEDRIVE_CLIENT_SECRET }}
    path: ${{ steps.build-args.outputs.filename }}
    refresh-token: ${{ secrets.ONEDRIVE_REFRESH_TOKEN }}
```

Put the following in your GitHub environment.

- Secrets
   - `ONEDRIVE_CLIENT_SECRET` with your client secret from Entra ID
   - `ONEDRIVE_REFRESH_TOKEN` with your refresh token
      - Should starts with to `M.C...`
- Variables
   - `ONEDRIVE_CLIENT_ID` with your application (client) ID from Entra ID

## Contributions

Like us? [Star](https://github.com/compulim/upload-to-onedrive/stargazers) us.

Want to make it better? [File](https://github.com/compulim/upload-to-onedrive/issues) us an issue.

Don't like something you see? [Submit](https://github.com/compulim/upload-to-onedrive/pulls) a pull request.
