# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This project is going to utilize semantic versioning for its tagging.
[semver.org](https://semver.org/)

General format:

**MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add funcionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI

### Considerations with the Terraform CLI changes

The Terraform CLI installation instructions have change due to gpg keyring changes. So wee needed to refer to the latest install CLI instructions via Terraform Documentation and change the scripting for install.

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Considerations for Linux Distribution

This project is built against Ubuntu.
Please consider checking your Linux Distribution and change accordingly to distribution needs.

[How to check OS version in Linux](https://www.cyberciti.biz/faq/find-linux-distribution-name-version-number/)

```sh
$ cat /etc/*-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=22.04
DISTRIB_CODENAME=jammy
DISTRIB_DESCRIPTION="Ubuntu 22.04.3 LTS"
PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

### Refactoring into Bash Scripts

While fixing the Terraform CLI gpg depreciation issues we notice that bash scripts step wre considerable amount more code. So we decided to create a bash script tp install the Terraform CLI.

The bash script is located here: [.bin/install-terraform-cli.sh](./bin/install-terraform-cli.sh) 

- This will keep the Gitpod Task file ([.gitpod.yml](.gitpod.yml))tidy.
- This allows us an easier to debug and execute manually Terraform CLI install
- This will allow better portability for other projects that need to install Terraform CLI.

#### Shebang

A Shebang (pronounced Sha-bang) tells the bash script what program will interpret the script. eg. #!/bin/bash`

ChatGPT recommended this format for bash: `#!/usr/bin/env bash`
- for portability for different OS distributions
- will search the users PATH for the bash executable

#### Execution considerations

When executing the bash script we can use the `./` shorthand notation to execute the bash script.

eg. `./bin/isntall-terraform-cli`

If we are using a script in .gitpod.yml we need to point the script to a program to interpret it.

eg. `source ./bin/install-terraform-cli`

#### Linux permissions considerations

In order to make our bash scripts executable we need to change linux permissions for the fix to be executable at the user mode

```sh
chmod u+x ./bin/install-tterraform-cli.sh
```

alternatively

```sh
chmod 744 ./bin/install-terraform-cli.sh
```

### Gitpod Lifecycle (Before, Init, Command)

We need to be careful when using the Init because it will not rerun if we restart an existing workspace.

### Working with Env Vars

We can list out all Environment variables (Env Vars) using the `env` command.

We can filter specific env vars using grep eg. `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set using `export HELLO='world'`

In the terminal we unset using `unset HELLO`

Wen can set an env var temporarily when just running an command

```sh
HELLO='world' ./bin/print_message
```

Within a bash script we can set an env without writing export eg.

```sh
#! /usr/bin/env bash

HELLO='world'

echo $HELLO
```

#### Printing Vars

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

When you open up new bash terminals in VSCode it will nit be aware of env vars that you have set in antorher window.

If you want env vars to persist across all future bash terminals that are opened you neet to set env vars in your bash profile eg. `.bash_profile`.

#### Persisting Env Vars in Gitpod

We can persist env varts into gitpod by storing them in gitpod secrets storage.

```
gp env HELLO='world'
```

All future workspaces launched will set the env vars for all bash terminals opened those workspaces.

You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.