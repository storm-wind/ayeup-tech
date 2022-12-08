# Azure CLI

The current version of the Azure CLI is **2.15.1**. For information about the latest release, see the [release notes](https://docs.microsoft.com/en-us/cli/azure/release-notes-azure-cli). To find your installed version and see if you need to update, run [az version](https://docs.microsoft.com/en-us/cli/azure/reference-index#az_version).

Note

The package for Azure CLI installs its own Python interpreter, and does not use the system Python.

On Ubuntu 20.04 (Focal), there is an `azure-cli` package with version `2.0.81` provided by the `focal/universe` repository. It's outdated and not recommended. If you already installed it, please remove it first by running `sudo apt remove azure-cli -y && sudo apt autoremove -y` before following the below steps to install the latest `azure-cli` package.

### Install

We offer two ways to install the Azure CLI with distributions that support `apt`: As an all-in-one script that runs the install commands for you, and instructions that you can run as a step-by-step process on your own.

### Install with one command

We offer and maintain a script which runs all of the installation commands in one step. Run it by using `curl` and pipe directly to `bash`, or download the script to a file and inspect it before running.

Important

This script is only verified for Ubuntu 16.04+ and Debian 8+. It may not work on other distributions. If you're using a derived distribution such as Linux Mint, follow the manual install instructions and perform any necessary troubleshooting.

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

```

### Manual install instructions

If you don't want to run a script as superuser or the all-in-one script fails, follow these steps to install the Azure CLI.

1.  Get packages needed for the install process:
    
    ```
    sudo apt-get update
    sudo apt-get install ca-certificates curl apt-transport-https lsb-release gnupg
    
    ```
2.  Download and install the Microsoft signing key:
    
    ```
    curl -sL https://packages.microsoft.com/keys/microsoft.asc |
        gpg --dearmor |
        sudo tee /etc/apt/trusted.gpg.d/microsoft.gpg > /dev/null
    
    ```
3.  Add the Azure CLI software repository:
    
    ```
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" |
        sudo tee /etc/apt/sources.list.d/azure-cli.list
    
    ```
4.  Update repository information and install the `azure-cli` package:
    
    ```
    sudo apt-get update
    sudo apt-get install azure-cli
    
    ```

Run the Azure CLI with the `az` command. To sign in, use the [az login](https://docs.microsoft.com/en-us/cli/azure/reference-index#az-login) command.

1.  Run the `login` command.
    
    ```
    az login
    
    ```
    
    If the CLI can open your default browser, it will do so and load an Azure sign-in page.
    
    Otherwise, open a browser page at [https://aka.ms/devicelogin](https://aka.ms/devicelogin) and enter the authorization code displayed in your terminal.
    
    If no web browser is available or the web browser fails to open, use device code flow with **az login --use-device-code**.
    
2.  Sign in with your account credentials in the browser.
    

To learn more about different authentication methods, see [Sign in with Azure CLI](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli).

### Troubleshooting

Here are some common problems seen when installing with `apt`. If you experience a problem not covered here, [file an issue on github](https://github.com/Azure/azure-cli/issues).

### No module issue on Ubuntu 20.04 (Focal)/WSL

If you installed `azure-cli` on `Focal` without adding the Azure CLI software repository in [step 3](#set-release) of the manual install instructions or using our [script](#install-with-one-command), you may encounter issues such as no module named 'decorator' or 'antlr4' as the package you installed is the outdated `azure-cli 2.0.81` from the `focal/universe` repository. Please remove it first by running `sudo apt remove azure-cli -y && sudo apt autoremove -y`, then follow the above [instructions](#install) to install the latest `azure-cli` package.

### lsb_release does not return the correct base distribution version

Some Ubuntu- or Debian-derived distributions such as Linux Mint may not return the correct version name from `lsb_release`. This value is used in the install process to determine the package to install. If you know the code name of the Ubuntu or Debian version your distribution is derived from, you can set the `AZ_REPO` value manually when [adding the repository](#set-release). Otherwise, look up information for your distribution on how to determine the base distribution code name and set `AZ_REPO` to the correct value.

### No package for your distribution

Sometimes it may be a while after a distribution is released before there's an Azure CLI package available for it. The Azure CLI designed to be resilient with regards to future versions of dependencies and rely on as few of them as possible. If there's no package available for your base distribution, try a package for an earlier distribution.

To do this, set the value of `AZ_REPO` manually when [adding the repository](#set-release). For Ubuntu distributions use the `bionic` repository, and for Debian distributions use `stretch`. Distributions released before Ubuntu Trusty and Debian Wheezy are not supported.

### Elementary OS (EOS) fails to install the Azure CLI

EOS fails to install the Azure cli because `lsb_release` returns `HERA`, which is the EOS release name. The solution is to fix the file `/etc/apt/sources.list.d/azure-cli.list` and change `hera main` to `bionic main`.

Original file contents:

```
deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ hera main

```

Modified file contents

```
deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ bionic main

```

### Proxy blocks connection

If you're unable to connect to an external resource due to a proxy, make sure that you've correctly set the `HTTP_PROXY` and `HTTPS_PROXY` variables in your shell. You will need to contact your system administrator to know what host(s) and port(s) to use for these proxies.

These values are respected by many Linux programs, including those which are used in the install process. To set these values:

```
# No auth
export HTTP_PROXY=http://[proxy]:[port]
export HTTPS_PROXY=https://[proxy]:[port]

# Basic auth
export HTTP_PROXY=http://[username]:[password]@[proxy]:[port]
export HTTPS_PROXY=https://[username]:[password]@[proxy]:[port]

```

Important

If you are behind a proxy, these shell variables must be set to connect to Azure services with the CLI. If you are not using basic auth, it's recommended to export these variables in your `.bashrc` file. Always follow your business' security policies and the requirements of your system administrator.

You may also want to explicitly configure `apt` to use this proxy at all times. Make sure that the following lines appear in an `apt` configuration file in `/etc/apt/apt.conf.d/`. We recommend using either your existing global configuration file, an existing proxy configuration file, `40proxies`, or `99local`, but follow your system administration requirements.

```
Acquire {
    http::proxy "http://[username]:[password]@[proxy]:[port]";
    https::proxy "https://[username]:[password]@[proxy]:[port]";
}

```

If your proxy does not use basic auth, **remove** the `[username]:[password]@` portion of the proxy URI. If you require more information for proxy configuration, see the official Ubuntu documentation:

- [apt.conf manpage](http://manpages.ubuntu.com/manpages/bionic/en/man5/apt.conf.5.html)
- [Ubuntu wiki - apt-get howto](https://help.ubuntu.com/community/AptGet/Howto#Setting_up_apt-get_to_use_a_http-proxy)

In order to get the Microsoft signing key and get the package from our repository, your proxy needs to allow HTTPS connections to the following address:

- `https://packages.microsoft.com`

### CLI fails to install or run on Windows Subsystem for Linux

Since [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/about) is a system call translation layer on top of the Windows platform, you might experience an error when trying to install or run the Azure CLI. The CLI relies on some features that may have a bug in WSL. If you experience an error no matter how you install the CLI, there's a good chance it's an issue with WSL and not with the CLI install process.

To troubleshoot your WSL installation and possibly resolve issues:

- If you can, run an identical install process on a Linux machine or VM to see if it succeeds. If it does, your issue is almost certainly related to WSL. To start a Linux VM in Azure, see the [create a Linux VM in the Azure Portal](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal) documentation.
- Make sure that you're running the latest version of WSL. To get the latest version, [update your Windows 10 installation](https://support.microsoft.com/help/4027667/windows-10-update).
- Check for any [open issues](https://github.com/Microsoft/WSL/issues) with WSL which might address your problem. Often there will be suggestions on how to work around the problem, or information about a release where the issue will be fixed.
- If there are no existing issues for your problem, [file a new issue with WSL](https://github.com/Microsoft/WSL/issues/new) and make sure that you include as much information as possible.

If you continue to have issues installing or running on WSL, consider [installing the CLI for Windows](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows).

### Update

The CLI provides an in-tool command to update to the latest version:

```
az upgrade

```

Note

The `az upgrade` command was added in version 2.11.0 and will not work with versions prior to 2.11.0.

This command will also update all installed extensions by default. For more `az upgrade` options, please refer to the [command reference page](https://docs.microsoft.com/en-us/cli/azure/reference-index#az_upgrade).

You can also use `apt-get upgrade` to update the CLI package.

```
sudo apt-get update && sudo apt-get upgrade

```

Note

This command upgrades all of the installed packages on your system that have not had a dependency change. To upgrade the CLI only, use `apt-get install`.

```
sudo apt-get update && sudo apt-get install --only-upgrade -y azure-cli

```

### Uninstall


1.  Uninstall with `apt-get remove`:
    
    ```
    sudo apt-get remove -y azure-cli
    
    ```
2.  If you don't plan to reinstall the CLI, remove the Azure CLI repository information:
    
    ```
    sudo rm /etc/apt/sources.list.d/azure-cli.list
    
    ```
3.  If you use no other packages from Microsoft, remove the signing key:
    
    ```
    sudo rm /etc/apt/trusted.gpg.d/microsoft.gpg
    
    ```
4.  Remove any unneeded packages:
    
    ```
    sudo apt autoremove
    
    ```

---


### Create a Vault in Azure using CLI

- Make sure your logged into Azure first
- Have a storage acocunt created
- Check appropriate **resource group**

`az keyvault create --name "enainfraterraformvault" --resource-group "ena-infra-terraform" --location australiaeast`

- `az` is azure cli command line
- `keyvault create --name "enainfraterraformvault"` is the name of keyvault to create
- `--resource-group "ena-infra-terraform"` is the name of resource group, this is very important.
- `--location` australiaeast` location of vault to be located.

* * *

### Reference Access key and export as environment variable

This allows you to export Access key for Azure service which is stored in an Azure Vault without having the Access key saved to disk and increasing the security risk.

Make sure your logged in to Azure by running `az login` from the cli and follow on screen instructions.

```
export ARM_ACCESS_KEY=$(az keyvault secret show --name enainfraterraform --vault-name enainfraterraformvault --query value -o tsv)
```

- ARM\_ACCESS\_KEY is env variable name
- The rest refeferences the vault

* * *

### List all Locations in Azure in Table format

This is how you list locations in Azure, there are a few output formats, like **json**, **yaml** as examples.

`az account list-locations --out table`

Output is something like this,

```
DisplayName               Name                 RegionalDisplayName
------------------------  -------------------  -------------------------------------
East US                   eastus               (US) East US
East US 2                 eastus2              (US) East US 2
South Central US          southcentralus       (US) South Central US
West US 2                 westus2              (US) West US 2
Australia East            australiaeast        (Asia Pacific) Australia East
Southeast Asia            southeastasia        (Asia Pacific) Southeast Asia
North Europe              northeurope          (Europe) North Europe
UK South                  uksouth              (Europe) UK South
West Europe               westeurope           (Europe) West Europe
Central US                centralus            (US) Central US
North Central US          northcentralus       (US) North Central US
West US                   westus               (US) West US
South Africa North        southafricanorth     (Africa) South Africa North
Central India             centralindia         (Asia Pacific) Central India
East Asia                 eastasia             (Asia Pacific) East Asia
Japan East                japaneast            (Asia Pacific) Japan East
Korea Central             koreacentral         (Asia Pacific) Korea Central
Canada Central            canadacentral        (Canada) Canada Central
France Central            francecentral        (Europe) France Central
Germany West Central      germanywestcentral   (Europe) Germany West Central
Norway East               norwayeast           (Europe) Norway East
Switzerland North         switzerlandnorth     (Europe) Switzerland North
UAE North                 uaenorth             (Middle East) UAE North
Brazil South              brazilsouth          (South America) Brazil South
Central US (Stage)        centralusstage       (US) Central US (Stage)
East US (Stage)           eastusstage          (US) East US (Stage)
East US 2 (Stage)         eastus2stage         (US) East US 2 (Stage)
North Central US (Stage)  northcentralusstage  (US) North Central US (Stage)
South Central US (Stage)  southcentralusstage  (US) South Central US (Stage)
West US (Stage)           westusstage          (US) West US (Stage)
West US 2 (Stage)         westus2stage         (US) West US 2 (Stage)
Asia                      asia                 Asia
Asia Pacific              asiapacific          Asia Pacific
Australia                 australia            Australia
Brazil                    brazil               Brazil
Canada                    canada               Canada
Europe                    europe               Europe
Global                    global               Global
India                     india                India
Japan                     japan                Japan
United Kingdom            uk                   United Kingdom
United States             unitedstates         United States
East Asia (Stage)         eastasiastage        (Asia Pacific) East Asia (Stage)
Southeast Asia (Stage)    southeastasiastage   (Asia Pacific) Southeast Asia (Stage)
East US 2 EUAP            eastus2euap          (US) East US 2 EUAP
West Central US           westcentralus        (US) West Central US
South Africa West         southafricawest      (Africa) South Africa West
Australia Central         australiacentral     (Asia Pacific) Australia Central
Australia Central 2       australiacentral2    (Asia Pacific) Australia Central 2
Australia Southeast       australiasoutheast   (Asia Pacific) Australia Southeast
Japan West                japanwest            (Asia Pacific) Japan West
Korea South               koreasouth           (Asia Pacific) Korea South
South India               southindia           (Asia Pacific) South India
West India                westindia            (Asia Pacific) West India
Canada East               canadaeast           (Canada) Canada East
France South              francesouth          (Europe) France South
Germany North             germanynorth         (Europe) Germany North
Norway West               norwaywest           (Europe) Norway West
Switzerland West          switzerlandwest      (Europe) Switzerland West
UK West                   ukwest               (Europe) UK West
UAE Central               uaecentral           (Middle East) UAE Central
Brazil Southeast          brazilsoutheast      (South America) Brazil Southeast
```

* * *



---

## Azure Storage using CLI


### Upload a file to blob container in Azure

This command will allow you to upload files to a specific blob container in Azure

```
az storage blob upload --account-name enainfraterraform --container-name tfstate --name helloworld-test --file helloworld-test --auth-mode key --account-key $ARM_ACCESS_KEY
```

- `az storage blob upload --account-name` storage account to upload to
- `--container-name` the blob you want to upload into
- `--name` name to call when uploaded
- `--file` the actual filename
- `--auth-mode key` you need to setup an env variable with access key first to use this option
- `--account-key $ARM_ACCESS_KEY` name of env variable you've used to store the access key in

If successful you will get some like this,
```
Finished[#############################################################]  100.0000%
{
  "etag": "\"0x8D85906BA9BD4E6\"",
  "lastModified": "2020-09-14T23:34:30+00:00"
}
```
* * *
### Download a file from a blob container or collection of files
To download files recursively, here is how you do it.
```
az storage blob download-batch --account-name enainfraterraform -d . -s tfstate --pattern test/* --auth-mode key --account-key $ARM_ACCESS_KEY
```


---


## Quickstart - Set and retreive a secret from Azure Key Vault

### Prerequisites

- Use [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/key-vault/secrets/../../cloud-shell/quickstart) using the bash environment.
    
    [![Embed launch](:/1b8d8a3b92704ecdb03d32d77368ed59 "Launch Azure Cloud Shell")](https://shell.azure.com)
    
- If you prefer, [install](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) Azure CLI to run CLI reference commands.
    
    - If you're using a local install, sign in with Azure CLI by using the [az login](https://docs.microsoft.com/en-us/cli/azure/reference-index#az-login) command. To finish the authentication process, follow the steps displayed in your terminal. See [Sign in with Azure CLI](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli) for additional sign-in options.
    - When you're prompted, install Azure CLI extensions on first use. For more information about extensions, see [Use extensions with Azure CLI](https://docs.microsoft.com/en-us/cli/azure/azure-cli-extensions-overview).
    - Run [az version](https://docs.microsoft.com/en-us/cli/azure/reference-index?#az_version) to find the version and dependent libraries that are installed. To upgrade to the latest version, run [az upgrade](https://docs.microsoft.com/en-us/cli/azure/reference-index?#az_upgrade).

- This quickstart requires version 2.0.4 or later of the Azure CLI. If using Azure Cloud Shell, the latest version is already installed.

### Create a resource group

A resource group is a logical container into which Azure resources are deployed and managed. The following example creates a resource group named *ContosoResourceGroup* in the *eastus* location.

Azure CLI

```
az group create --name "ContosoResourceGroup" --location eastus

```

### Create a Key Vault

Next you will create a Key Vault in the resource group created in the previous step. You will need to provide some information:

- For this quickstart we use **Contoso-vault2**. You must provide a unique name in your testing.
- Resource group name **ContosoResourceGroup**.
- The location **East US**.

Azure CLI

```
az keyvault create --name "Contoso-Vault2" --resource-group "ContosoResourceGroup" --location eastus

```

The output of this cmdlet shows properties of the newly created Key Vault. Take note of the two properties listed below:

- **Vault Name**: In the example, this is **Contoso-Vault2**. You will use this name for other Key Vault commands.
- **Vault URI**: In the example, this is [https://contoso-vault2.vault.azure.net/](https://contoso-vault2.vault.azure.net/). Applications that use your vault through its REST API must use this URI.

At this point, your Azure account is the only one authorized to perform any operations on this new vault.

### Add a secret to Key Vault

To add a secret to the vault, you just need to take a couple of additional steps. This password could be used by an application. The password will be called **ExamplePassword** and will store the value of **hVFkk965BuUv** in it.

Type the commands below to create a secret in Key Vault called **ExamplePassword** that will store the value **hVFkk965BuUv** :

Azure CLI

```
az keyvault secret set --vault-name "Contoso-Vault2" --name "ExamplePassword" --value "hVFkk965BuUv"

```

You can now reference this password that you added to Azure Key Vault by using its URI. Use **'https://Contoso-Vault2.vault.azure.net/secrets/ExamplePassword'** to get the current version.

To view the value contained in the secret as plain text:

Azure CLI

```
az keyvault secret show --name "ExamplePassword" --vault-name "Contoso-Vault2"

```

Now, you have created a Key Vault, stored a secret, and retrieved it.

### Clean up resources

Other quickstarts and tutorials in this collection build upon this quickstart. If you plan to continue on to work with subsequent quickstarts and tutorials, you may wish to leave these resources in place. When no longer needed, you can use the [az group delete](https://docs.microsoft.com/en-us/cli/azure/group) command to remove the resource group, and all related resources. You can delete the resources as follows:

Azure CLI

```
az group delete --name ContosoResourceGroup

```
