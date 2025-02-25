# Terraform Beginner Bootcamp 2023

## Semantic Versioning ! :mage:

This project is goint fo tutilie semantic versioning for its tagging
[semver.org](https://semver.org)

The general format:

**MAJOR.MINOR.PATCH**, eg. `1.0.1`Given a version number MAJOR.MINOR.PATCH, increment the:

MAJOR version when you make incompatible API changes
MINOR version when you add functionality in a backward compatible manner
PATCH version when you make backward compatible bug fixes
- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI

### Considerations with the Terraform CLI changes

The Terraform CLI installation instructions have chaged due to gpg keyring changes. So we needed to refer to latest install instructions via Terraform Docmentation and change the scriptiong for install. 

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)


### Consideration for Linux Distribution

This project is built again Linux distribution please consider checking your Linux Distribution and change accordingly to distribution needs.

[How To Check OS Version in Linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

```sh
$ cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.5 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.5 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
$ 
```
### Refactoring into Bash Scripts

While fixing the Terraform CLI gpg depreciation issues we notice that bash scripts steps weree a considerable amput more code. so we decided to create a bash script to install thee Terraform CLI

This bash script is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli.sh)

- This will keep the Gitpod Task File ([.gitpod.yml](.gitpod.yml)) tidy
- This allow us an easier to debug and execute manually Terraform CLI install
- This will allow better portability for other project that need to install CLI

### Shebang Consideration

A Shebang (pronounced Sha-bang) tells the bash script what program theat will interpret  the script. eg. `#!/bin/bash`

ChatGPT recommended this format for bash: `#!/usr/bin/env bash` 
- for portability for different OS distributions
- will search the user's PATH for the bash executable

#### Execution Considuration

When executing the bash script we can use the `./` shorthand notation to execute the bash script.
[Shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))

eg. `./bin/install_terraform_cli`

If we are using a script in .gitpot.yml we need to point the script to a program to interpret it.

eg. `source ./bin/install_terraform_cli`

#### Linux Permission Consideration

In order to make our bash scripts executable we need to change linux permission for the file to be executable at the user mode

```sh
chmod u+x ./bin/install_terraform_cli
```
Alternatively, we can also use this command
```sh
chmod 744 ./bin/install_terraform_cli
```
You can read more about the file permissions [here](https://en.wikipedia.org/wiki/Chmod)


### Github LIfecycle (Before, Init, Command)

We need to be careful when using the Init because it will not rerun if we restart an exiting workspace. Read more information [here](https://www.gitpod.io/docs/configure/workspaces/tasks).


### Working With Env

We can list out all Environment Variables (Env Vars) using the `env` command

We can filter specific specific env vars using grep eg. `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set using `expot HELLO='world'`

We can set an env vars temporarily when just running a command

```sh
$ HELLO='world' ./bin/print_message
$
```

Within a bash script we can set env without writing export eg.

```sh
#!/usr/bin/env bash

HELLO='world

echo $HELLO
```

#### Printing Vars

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

When you open new bash terminal in VSCode, it will not be aware of env vars that you have set in another window.

If you want the Env Vars to persist across all future terminals that are open you need to set env vars in your bash profile. eg. `.bash_profile`

#### Persisting Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod Secrets Storage.

```
gp env HELLO='world'
```

All future workspaces lauched willset the env vars for all bash terminals in those workspaces.

You can also set env vars in the `.gitpod.yml` but this can only contains non-sensitive var


### AWS CLI Installation
AWS CLI is install for this project via the bas script [`./bin/install_aws_cli`](./bin/install_aws_cli)

[Getting Started Insatall ()](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

We can check if our AWS Credentials is configured correctly by running the following AWS CLI command

```sh
aws sts get-caller=identity
```
[AWS CLI Env Vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

If it is successful you should see a json payload return that looks like this:

```json
{
    "UserId": "A9HAZTTLB7WQRWOU63N2R",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/Administrator"
}
```

We'll need to generate AWS CLI Credential from the AIM User in order to use the AWS CLI.