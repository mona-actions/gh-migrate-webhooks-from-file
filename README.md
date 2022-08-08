# Migrate webooks to destination with input file

**GitHub CLI** extensions are repositories that provide additional `gh` commands, and this **GitHub CLI** extension complements [**gh-migrate-webhook-secrets**](https://github.com/mona-actions/gh-migrate-webhook-secrets) to take that exported file in `*.csv` file and migrates Webhooks to target organization

## GHES Compatibility
The **gh-migrate-webhooks-from-file** extension supports the following versions of GitHub Enterpise Server (GHES):

- Supported: >= 2.20
- Not Supported: <= v2.19

*It should be noted that support for versions < 3.1 is limited.*

## Prerequisites

- You need to have a CSV file created from [**gh-migrate-webhook-secrets**](https://github.com/mona-actions/gh-migrate-webhook-secrets)
- Operating system that can run shell scripts (*bash/sh*)
- **GitHub CLI** installed by following this documentation: <https://github.com/cli/cli#installation>
- **jq** command-line JSON parser: <https://stedolan.github.io/jq/>
- Your destination organization have all repository names identical to repositories in source organization
- You have ownership/admin permissions in the destination GitHub GHES/GHEC where you want to migrate to
- You need to create a GitHub **Personal Access Token** with all `repo` permission `admin:org` permission in your destination URL and authenticate for the destionation organization if the org is behind SAML/SSO

You need to either export these environment variables.

| Environment Variable name | Value                                                                                       |
| ------------------------- | ------------------------------------------------------------------------------------------- |
| DEST_URL | GitHub URL or GHES URL where you want to migrate. Defaults to `https://github.com` |
| GH_TOKEN_DEST | GitHub Personal Access Token (PAT) for your destination organization with all `repo` permission `admin:org` permission  |
| DEST_ORG | Destination organization name where you want to migrate to  |

Or the script will prompt you to put in the relevant information.

## CLI options

```text
Usage: gh migrate-webhooks-from-file [options]
Options:
    -h, --help                    : Show script help
    -du, --destination-url        : Set destination GitHub URL (e.g. https://github.example.com) Looks for DEST_URL environment
                                    variable if omitted or defaults to https://github.com                                 
    -dt, --destination-token      : Set Personal Access Token for destination organization with repo scope - Looks for GH_TOKEN_DEST environment
                                    variable if omitted
    -do, --destination-org        : Set destination organization to migrate to - Looks for DEST_ORG environment
                                    variable if omitted
Description:
gh migrate-webhooks-from-file migrates the webhooks to destination organization provided that an input file from mapping file is created
Example:
  gh migrate-webhooks-from-file -do final-destination-org -dt ABCDEFG1234567
```

## How to run

Make sure you followed prerequisites and then follow these instructions.

### Step 1: Generate input file containing webhooks to migrate from gh-migrate-webhook secrets

You need to generate a CSV file created from [**gh-migrate-webhook-secrets**](https://github.com/mona-actions/gh-migrate-webhook-secrets)

Copy and paste that file into the directory where you want to run this `gh-migrate-webhooks-from-file` script

### Step 2: Install GitHub extension

```sh
gh extension install mona-actions/gh-migrate-webhooks-from-file
```

### Step 2: Run gh migrate-collaborator-permission

```sh
 gh migrate-webhooks-from-file -do final-destination-org -dt ABCDEFG1234567
```