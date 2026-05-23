# CreateBackup.ps1

## Overview

`CreateBackup.ps1` creates a timestamped `.zip` backup file from an application installation directory.

The script reads the following environment variables:

- `APPLICATIONNAME`
- `SERVICENAME`
- `APPLICATIONINSTALLATIONDIRECTORY`
- `BACKUPFOLDERDIRECTORY`

## What the script does

1. Reads required values from environment variables.
2. Creates a temporary folder under the backup directory.
3. Copies installation files to the temporary folder.
4. Removes specific unneeded files/folders from the temporary folder.
5. Creates a ZIP file named like `bkp_<service>_<date>.zip`.
6. Deletes the temporary folder.

## Important note for future use

This script was created for a specific type of backup.

The section marked as:

`#Remove unneeded folders and files`

may need to be updated for future use, depending on your application structure and which files/folders should be excluded.

## How to use

### Local execution

1. Set the required environment variables.
2. Run the script in PowerShell:

```powershell
.\CreateBackup.ps1
```

### Azure DevOps usage

You can run this script in an Azure DevOps pipeline using a PowerShell task.

Example:

```yaml
trigger: none

pool:
  vmImage: windows-latest

steps:
- checkout: self

- task: PowerShell@2
  displayName: Run backup script
  inputs:
    targetType: filePath
    filePath: CreateBackup.ps1
    pwsh: false
  env:
    APPLICATIONNAME: $(APPLICATIONNAME)
    SERVICENAME: $(SERVICENAME)
    APPLICATIONINSTALLATIONDIRECTORY: $(APPLICATIONINSTALLATIONDIRECTORY)
    BACKUPFOLDERDIRECTORY: $(BACKUPFOLDERDIRECTORY)
```

### Azure DevOps variable setup

Define these pipeline variables (or variable group values):

- `APPLICATIONNAME`
- `SERVICENAME`
- `APPLICATIONINSTALLATIONDIRECTORY`
- `BACKUPFOLDERDIRECTORY`

## Output

The script generates a ZIP file in `BACKUPFOLDERDIRECTORY` with the format:

`bkp_<serviceName>_<ddMMyyyy.hhmmss>.zip`
