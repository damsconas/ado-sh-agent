# Self-Hosted Azure DevOps Agent Deployment Script

This repository contains a PowerShell script to deploy a self-hosted Azure DevOps agent on a Windows machine. The script automates the process of downloading, extracting, and configuring the agent, making it easy to set up a self-hosted agent for your Azure DevOps pipelines.

## Prerequisites

Before running the script, ensure you have the following:

1. **Azure DevOps Organization URL**: The URL of your Azure DevOps organization (e.g., `https://dev.azure.com/<organization>`).
2. **Personal Access Token (PAT)**: A PAT with sufficient permissions to register an agent. You can generate a PAT from your Azure DevOps account under **User Settings > Personal Access Tokens**.
3. **Agent Name**: A name for the agent you are deploying.
4. **Pool Name**: The name of the agent pool where the agent will be registered.

## Script Overview

The script performs the following steps:

1. **Downloads the Agent Package**: Downloads the latest self-hosted agent package from the specified URL.
2. **Extracts the Agent Package**: Extracts the downloaded package to a specified directory.
3. **Configures the Agent**: Runs the configuration script with the provided parameters to register the agent with your Azure DevOps organization.

## Script Variables

- `$AgentPackageUrl`: The URL to download the self-hosted agent package.
- `$AgentPackagePath`: The temporary path where the agent package will be downloaded.
- `$AgentExtractPath`: The directory where the agent package will be extracted.
- `$OrganizationUrl`: The URL of your Azure DevOps organization.
- `$PAT`: Your Personal Access Token (PAT) for Azure DevOps.
- `$AgentName`: The name of the agent.
- `$PoolName`: The name of the agent pool.

## Usage

1. Clone this repository or download the script to your local machine.
2. Open the script in a text editor and update the following variables with your specific values:
   - `$OrganizationUrl`
   - `$PAT`
   - `$AgentName`
   - `$PoolName`
3. Run the script in PowerShell with administrative privileges.

```powershell
.\deploy-self-hosted-agent.ps1
```

## Example

Here’s an example of how to configure the script:

```powershell
# Define variables
$AgentPackageUrl = "https://vstsagentpackage.azureedge.net/agent/3.238.0/vsts-agent-win-x64-3.238.0.zip"
$AgentPackagePath = "$env:TEMP\agent.zip"
$AgentExtractPath = "C:\agent"

# Set Azure DevOps organization URL
$OrganizationUrl = "https://dev.azure.com/myorganization" 

# Set Personal Access Token 
$PAT = "mypersonalaccesstoken"

# Set agent name
$AgentName = "MyAgent01"

# Set pool name
$PoolName = "Default"

# Download agent package
Invoke-WebRequest -Uri $AgentPackageUrl -OutFile $AgentPackagePath

# Extract agent package
Expand-Archive -Path $AgentPackagePath -DestinationPath $AgentExtractPath

# Navigate to agent directory
Set-Location -Path $AgentExtractPath

# Run configuration script with parameters
& .\config.cmd --url $OrganizationUrl --token $PAT --pool $PoolName --agent $AgentName --unattended --runAsService --runAsAutoLogon
```

## Notes

- Ensure that the directory specified in `$AgentExtractPath` exists or the script will create it.
- The `--runAsService` flag configures the agent to run as a Windows service.
- The `--runAsAutoLogon` flag ensures the agent runs automatically upon system startup.

## Troubleshooting

- **Access Denied Errors**: Ensure you are running the script with administrative privileges.
- **Invalid PAT**: Verify that the PAT has the necessary permissions and has not expired.
- **Agent Registration Failure**: Check the agent logs in the `_diag` folder under the agent directory for more details.

## Contributing

If you have any suggestions or improvements, feel free to open an issue or submit a pull request. Contributions are welcome!
