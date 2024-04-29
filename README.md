# Terraform Bootcamp 

### Semantic Versioning :mage:

This Project is going to utilize semantic versioning for its tagging.
[semver.org](https://semver.org)
Given a version number **MAJOR.MINOR.PATCH**, increment the:

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

### Install the Terraform CLI

#### Considerations with the Terraform CLI changes
- The Terraform CLU installation instructions have changed due to gpg keyring changes. Referred to the latest install CLI instructions via Terraform Documentation and change the scripting for install.
    [Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

#### Considerations for Linux Distributions
- Run the command:
    `cat /etc/*release`

```yml
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=22.04
DISTRIB_CODENAME=jammy
DISTRIB_DESCRIPTION="Ubuntu 22.04.4 LTS"
PRETTY_NAME="Ubuntu 22.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.4 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

#### Refactoring into Bash Scripts
- While fixing the Terraform CLI gpg depreciation issues we noice that bash scripts steps were a considerable amount of more code. So we decided to create a bash script to install the Terraform CLI.
    - This will keep the Gitpod Task File tidy
    - This allows us an easier time to debug and execute manually Terraform CLI install
    - This will allow better portability for other projects that need to install Terraform CLI

#### Shebang
- A Shebang (pronounced Sha-bang) tells the bash script what program that the script will interpret. eg. `#!/bin/bash`
    [Shebang Docs](https://en.wikipedia.org/wiki/Shebang_(Unix))
- ChatGPT recommended this format for bash: `#!/usr/bin/env bash`
    - for portability for different OS distributions
    - will search the user's PATH for the bash executable

#### Execution Considerations    
- When executing the bash script we can use the `./` shorthand notation to execute the bash script.
- e.g `./bin/install_terraform_cli`
- If we are using a script in .gitpod.yml we need to point the script to a program to interpret it.
e.g `source ./bin/install_terraform_cli`

#### Linux Permissions Considerations
- Inorder to make our bash scripts executable we need to change linux permission for the fix to be executable at the user mode.
```sh
chmod u+x ./bin/install_terraform_cli
```
[Linux permisions](https://en.wikipedia.org/wiki/Chmod)

#### Gitpod Lifecycle (Before, Init Command)
- Need to be careful when using the init because it will not return if we restart an existing workspace
[GitPod Docs](https://www.gitpod.io/docs/configure/workspaces/tasks)