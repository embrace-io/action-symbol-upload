# embrace-io/action-symbol-upload

This action allows uploading of symbol files for your mobile applications to Embrace, as described in
[Symbolicating Crash Reports](https://embrace.io/docs/ios/open-source/symbolicating-crash-reports/).

To use, you need to obtain:

- Your 5-digit Embrace app ID.
- Your 32-digit Embrace symbol upload API key, from [API settings screen](https://dash.embrace.io/settings/organization/api).

You can store the app ID as a [repository (or organization) variable](https://docs.github.com/en/actions/learn-github-actions/variables#defining-configuration-variables-for-multiple-workflows), 
or hardcode it in the workflow directly.

For security reasons, you should store the API key in your repository (or organization) secrets; see [GitHub documentation](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions) 
for detailed information on how to use GitHub Secrets.

Also, make sure you enable dSYM generation as described here: https://embrace.io/docs/ios/faq/#dsym-generation-not-enabled

## Basic usage

```yaml
name: CI
on:
  push:
    branches: [master]
  pull_request:
  release:
    types: [released]
  workflow_dispatch:

jobs:
  build-my-app:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v4

      - run: |
          xcodebuild build ...

      - uses: embrace-io/action-symbol-upload@main
        with:
          app_id: "${{ vars.EMBRACE_APP_ID }}"
          api_token: "${{ secrets.EMBRACE_API_TOKEN }}"
```

## Advanced usage

- You can use [GitHub Environments](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment) and environment-specific variables to vary your app ID between development builds and released "production" applications.
