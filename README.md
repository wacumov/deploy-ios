# Deploy iOS
This reusable GitHub Actions workflow automates the process of building, signing, and deploying iOS apps to TestFlight.

This workflow is intended for projects that use [XcodeGen](https://github.com/yonaskolb/XcodeGen).

## Usage
To use this workflow, you need to call it from another workflow file in your repository (e.g., `.github/workflows/deploy-to-testflight.yml`):

```yaml
name: Deploy to TestFlight

on:
  workflow_dispatch:

jobs:
  deploy:
    uses: wacumov/deploy-ios/.github/workflows/deploy.yml@0.2.0
    with:
      app-name: "YourAppName"
      bundle-id: "com.yourcompany.yourapp"
      development-team: "YOUR_TEAM_ID"
      project-name: "YourProjectName"
      provisioning-profile-name: "YourProvisioningProfileName"
      scheme: "YourSchemeName"
      pre-build-commands: |
        echo "Running pre-build steps..."
        # Add any commands you need to run before the build
    secrets:
      appleid-email: ${{ secrets.APPLEID_EMAIL }}
      appleid-password: ${{ secrets.APPLEID_PASSWORD }}
      keychain-password: ${{ secrets.KEYCHAIN_PASSWORD }}
      mobileprovision-base64: ${{ secrets.MOBILEPROVISION_BASE64 }}
      p12-base64: ${{ secrets.P12_BASE64 }}
      p12-password: ${{ secrets.P12_PASSWORD }}
      personal-access-token: ${{ secrets.PAT }}
```

## Inputs
The following inputs are required to configure the deployment:

| Input | Description |
| --- | --- |
| `app-name`                  | The name of your application. |
| `bundle-id`                 | The bundle identifier of your application. |
| `development-team`          | Your Apple Developer Team ID. |
| `project-name`              | The name of your Xcode project (without the `.xcodeproj` extension). |
| `provisioning-profile-name` | The name of the provisioning profile to use for signing. |
| `scheme`                    | The Xcode scheme to build. |
| `pre-build-commands`        | Optional: A string of commands to be executed before the build process begins. Useful for custom setup. |

## Secrets
The following secrets must be configured in your repository's "Settings > Secrets and variables > Actions" for the workflow to run successfully:

| Secret | Description |
| --- | --- |
| `APPLEID_EMAIL` | Your Apple ID email address used for TestFlight uploads. |
| `APPLEID_PASSWORD` | An app-specific password for your Apple ID. **Do not use your actual Apple ID password.** |
| `KEYCHAIN_PASSWORD` | A password to create a temporary keychain on the GitHub Actions runner. |
| `MOBILEPROVISION_BASE64` | Your provisioning profile (`.mobileprovision` file) encoded in Base64. |
| `P12_BASE64` | Your distribution certificate (`.p12` file) encoded in Base64. |
| `P12_PASSWORD` | The password for your `.p12` certificate file. |
| `PAT` | Optional: A GitHub Personal Access Token (PAT) if your `pre-build-commands` need to access other private repositories. |

### How to generate Base64 encoded secrets:
To generate the Base64 encoded versions of your `.mobileprovision` and `.p12` files, you can use the following commands in your terminal:

**For the provisioning profile:**
```bash
base64 -i your_profile.mobileprovision | pbcopy
```

**For the certificate:**
```bash
base64 -i your_certificate.p12 | pbcopy
```

Paste the copied output as the value for the corresponding secret in GitHub.
