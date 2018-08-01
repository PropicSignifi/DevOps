# DevOps

Click To Cloud DevOps Toolbox

This DevOps toolbox provides some of our daily used scripts, most of which
are Salesforce platform related, typically wrappers of some `sfdx` commands.

## Prerequisite

The toolbox runs on a Unix shell. The usual `sh` or `bash` is sufficient.

Many of the commands require `sfdx`, which can be downloaded from the
Salesforce official site: https://developer.salesforce.com/tools/sfdxcli

Some special commands may have some other dependencies, which will be
listed in other detailed pages.

One feature of the toolbox is the bash auto completion, which requires the
package `bash-completion`. This can be downloaded from package managers
such as `brew` and `apt-get`.

## Installation

A recommended way of installation is cloning this repository, then include
the repository in your shell environment path.

```bash

git clone https://github.com/Click-to-Cloud/DevOps.git

echo "PATH+=:$PWD/DevOps" >> ~/.bashrc

```

Then restarting your bash, or running `source ~/.bashrc`, will get things
ready.

To include the bash completion feature, append this code to your .bashrc

```bash

source $PWD/DevOps/bash_completion.d/dev

```

## Repository based projects

All the Salesforce commands requires login information to operate on an
org. Fortunately, `sfdx` allows setting org alias which we can make use of.
To make things convenient, we are using org alias base on the project
directory name. For example, after cloning the repository 'ctcproperty', all
Salesforce related commands running inside the repository, including its
subdirectories, will be applied to the org with alias 'ctcproperty'.

The first time operating under a new project, requires an OAuth login to link
the OAuth tokens with the alias. Simply `cd` to your project directory and
run `dev login`, which will popup a new window for the login. After the
login, the alias and login information will be saved by `sfdx`. Next time
if we want to link the alias to another login user, we can run `dev login`
again to change the login account.

Since `sfdx` has been saving our login information and alias names, we can
save our time from running `dev login` all the time whenever we want to
change the login user under the alias. So instead of running `dev login`,
`dev alias <username>` will also do the same magic. To show all alias and
its associated login user, we can run `dev alias` without a third argument.

