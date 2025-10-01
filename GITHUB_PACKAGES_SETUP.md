# GitHub Packages Alternative Setup

If you want to use GitHub Packages instead of NPM:

## 1. Update package.json for GitHub Packages

```json
{
  "name": "@aravindkarteekr/react-cache-clean",
  "publishConfig": {
    "registry": "https://npm.pkg.github.com/"
  }
}
```

## 2. Update workflow permissions

```yaml
name: Publish Package
on:
  release:
    types: [created]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: "18"
          registry-url: "https://npm.pkg.github.com/"
          scope: "@aravindkarteekr"
      - run: npm install
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{secrets.GITHUB_TOKEN}}
```

## 3. Repository Settings

- Go to repo Settings → Actions → General
- Set "Workflow permissions" to "Read and write permissions"
- Enable "Allow GitHub Actions to create and approve pull requests"

## 4. Organization Requirements

- If this is an organization repo, you need admin access
- Personal repos should work with the above setup
