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

#### Working with env vars
**env command**
- We can list out all Environment Variables (Env vars) using the 'env' command
- We can filter specific env vars using grep e.g `env | grep AWS_`

#### Setting and Unsetting env vars
- In the terminal we can set using `export HELLO='world'`
- We can unset using `unset HELLO`
- We can set an env var temporarily when running the command:
```sh
HELLO='world' ./bin/print_message
```
- Within a bash script we can set env without writing export e.g
```sh
#!/usr/bin/env bash

HELLO='world'
echo $HELLO
```

#### Printing vars
- We can print an env var using echo e.g `echo $HELLO`

#### Scoping of env vars
- When you open bash terminals in VSCode it will not be aware of env vars that you have set in another window
- If you want env vars to persist across all future bash terminals that are open you need to set env vars in your bash profile. e.g `.bash_profile`

#### Persisting env vars in Gitpod
- We can persist env vars into gitpod by storing them in Gitpod Secrets Storage.

```
gp env HELLO='world'
```
- All future workspaces launched will set the env vars for all bash terminals opened in those workspaces.
- You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars

#### AWS CLI Installation
- AWS CLI is installed for the project via the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)

[Getting started install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

- We can check if our aws credentials is configured correctly by running the following AWS CLI coomand:
```
aws sts get-caller-identity
```
[AWS CLI env vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

- Need to generate AWS CLI credits from IAM user inorder to use AWS CLI

### Terraform Basics
#### Terraform Registry
- Terraform sorces their providers and modules from the Terraform registry which is located at [registry.terraform.io](https://registry.terraform.io/)

- **Providers** is an interface to APIs that will allow you to create resources in terraform
- **Modules** are a way to make large amounts of terraform code modular, portable and easily shared.

[Random Terraform Provider](https://registry.terraform.io/providers/hashicorp/random)

#### Terraform Console
- We can see a list of all the Terraform commands by simply typing `terraform`

#### Terraform Init 
- At the start of a new terraform project we will run `terraform init` to download the binaries for the terraform providers that we'll use in this project
`terraform init`

#### Terraform Plan
- This will generate out a changeset, about the state of our infrastructure and what will be changed
- We can output this changeset i.e. "plan" to be passed to an apply, but often you can ignore outputting.
`terraform plan`

#### Terraform Apply
- This will run a plan and pass the changes to be executed by terraform. Apply shoul prompt us yes or no.
`terraform apply`
- If we want to automatically approve an apply we can provide the auto approve flag eg. `terraform apply --auto-approve`

#### Terraform Lock Files
`.terraform.lock.hcl`
- Contains the locked versioning for the providers or modules that should be used with this project
- The Terraform Lock File **should be committed to your Version Control System (VCS) eg. Github**

#### Terraform State Files
`.terraform.tfstate`
- Contains infromation about the current state of your infrastructure
- This file **should not be committed to your Version Control System (VCS) eg. Github**
- This file can contain sensitive data. If you lose this file, you lose knowing the state of your infrastructure

`.terraform.tfstate.backup`
- This is the previous state file state.

### Terraform Directory
- `.terraform` directory contains binaries terraform providers.