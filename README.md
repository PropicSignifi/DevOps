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
the repository in your shell environment path. In this example, we are
using `~/.bashrc` as the shell startup script, though some shell may use other
start up script, e.g. `~/.profile` or `~/.bash_profile`.

```bash

git clone https://github.com/Click-to-Cloud/DevOps.git

echo "PATH+=:$PWD/DevOps" >> ~/.bashrc

```

Then restarting your bash, or running `source ~/.bashrc`, will get things
ready.

To include the bash completion feature, append this code to your bashrc.

```bash

echo "source $PWD/DevOps/bash_completion.d/dev" >> ~/.bashrc

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

## Commands

### dev login [url]

Do OAuth login and associate the login information with the current project.

An optional third argument `url` is available, which can be only either
'test.salesforce.com' or 'login.salesforce.com'. If this argument is ommitted,
the login will go to 'test.salesforce.com' by default.

### dev open [alias|username]

Open the org in the default browser without using password. If the `alias` or
`username` is provided, it would open the org according to the provided login
information, otherwise, it would choose the alias on the project to open.

The `alias` or the `username` must have been used in the `dev login` at least
once. More specifically, after running `dev login` on the project
'ctcproperty' using the username 'sam@example.com', the OAuth information of
'sam@example.com' will be saved, and 'ctcproperty' will be linked to the
username 'sam@example.com'. In this case we can use `dev open ctcproperty` or
`dev open sam@example.com` to open the org on the browser. Alternatively, if
we have already navigated inside the 'ctcproperty' project directory, we can
save the third argument, running `dev open` only.

### dev alias [username]

If the `username` is not provided, the command will list all alias with its
associated username.

If a `username` is given, the command will replace the associated username
of the project with the given `username`.

### dev compile \<class\_files|page\_files|trigger\_files\>

Compile the class/page/trigger files and deploy it to the associated org.

A file can be any of these types: Apex Class, Apex Page, Apex Trigger.

```bash

dev compile Query.cls TriggerOnAccount.trigger HomePage.page

```

By default, it will look for the files using the relative pathname. If not
found it will look for the files in the `src/classes`, `src/pages` or the
`src/triggers` directories.

It is required that the related meta XML files are located in the same
directory.

For example, calling `dev compile Query.cls`, the script will try to find
`Query.cls` in the current directory. If not found, it will look for `Query.cls`
in the directory `src/classes`. If, for example, finding the `Query.cls` in the
current path, the script will use the `Query.cls-meta.xml` file in the same
directory for the deployment. If the `Query.cls-meta.xml` file is missing, the
deployment will fail.

### dev runtest [test\_classes ...]

Execute unit tests. If `test_classes` is provided, it will execute all test
code within those classes. Otherwise, it will execute all test code in all
classes.

### dev query \<soql\_expression\>

Execute an SOQL query and print the result to standard output.

To avoid some of the characters, e.g. single quote, less than sign, being
parsed as bash operators, it is recommended that the `soql_expression`
is surrounded by double quotes.

```bash

dev query select id from account

```

```bash

dev query "select id from account where name = 'Jack' and createddate >= today"

```

### dev execute

Execute anonymous apex code from the standard input.

If the command is `dev execute`, it reads the user terminal input as the
standard input. In this case, we can type multiple lines of apex codes,
then type CTRL-D indicating the end of file to finish.

Alternatively, we can write the apex code in a file, then pass the file
name as an argument.

```bash

dev execute test.apex

```

### dev data \<object\_name\> \<record\_id\>

Print detail of a record to standard output.

### dev fields \<object\_name\>

Print the fields of an object to standard output.

### dev schema \<object\_name\>

Print the full schema information of an object to standard output.

### dev debug \<apex\_expression\>

Print the return value of an apex expression to standard output.

```bash

dev debug "UserInfo.getUserId()"

```
