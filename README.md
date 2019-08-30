# DevOps

Click To Cloud DevOps Toolbox

This DevOps toolbox provides some of our daily used scripts, most of which
are Salesforce platform related, typically wrappers of some `sfdx` commands.

## Features

### Shorter subcommands

For example, instead of

```bash
sfdx force:apex:execute -u myOrgAlias -f ~/test.apex
```

We simply do

```bash
dev execute ~/test.apex
```

### Omit user name or org alias

No need to worry about the long usernames or alias. It automatically uses
the name of the git repository as an alias.

### Bash auto completion

For example, `dev alias ^TAB` will list all user name or alias you have saved.

```
$ dev alias
admin@my-dev.com.au                  henry@my-dev.com.test
```

## Prerequisite

The toolbox runs on a Unix shell. The usual `sh` or `bash` is sufficient.

Many of the commands require `sfdx`, which can be downloaded from the
Salesforce official site: https://developer.salesforce.com/tools/sfdxcli

The optional bash auto completion requires the package `bash-completion`,
which can be downloaded from some package managers, for example `brew` or
`apt-get`.

```
brew install bash-completion
```

## Installation

A recommended way of installation is cloning this repository, and including
the repository in your shell environment path. In this example, we are
using `~/.bashrc` as the shell startup script, though some shell may use other
startup script, e.g. `~/.profile` or `~/.bash_profile`.

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

Suppose you are working on a git repository, run `dev login` if you haven't
done so. After connecting to a Salesforce org, everything command you use
inside the git repository will be against the org you just logged in.

When you want to switch to another org for the same repository, you can do
another `dev login`. Alternatively, if a username has been used to log in
previously, you can run `dev alias yourusername@domain.com`. You can check
all available usernames by auto completing the third argument of
`dev alias ^TAB`.

## Commands

### dev login [url]

Do OAuth login and associate the login information with the current repository.

An optional third argument `url` is available, which can be only either
'test.salesforce.com' or 'login.salesforce.com'. If this argument is ommitted,
the login will go to 'test.salesforce.com' by default.

The third argument can be auto completed.

```bash
dev login login.salesforce.com
```

```bash
dev login
```

### dev open [alias|username]

Open the org in the default browser without using password. If the `alias` or
`username` is provided, it would open the org according to the provided login
information, otherwise, it would choose the alias on the repository to open.

The `alias` or the `username` must have been used in the `dev login` at least
once. More specifically, after running `dev login` on the repository
'Query' using the username 'sam@example.com', the OAuth information of
'sam@example.com' will be saved, and 'Query' will be linked to the username
'sam@example.com'. In this case we can use `dev open Query` or
`dev open sam@example.com` to open the org on the browser. Alternatively, if
we have already navigated ourselves inside the 'Query' repository , we can
omit the third argument, running `dev open` only.

### dev alias [username]

If the `username` is not provided, the command will list all alias with its
associated username.

If a `username` is given, the command will replace the associated username
of the project with the given `username`.

The third argument can be auto completed.

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

The arguments can be auto completed.

### dev runtest [test\_classes ...]

Execute unit tests. If `test_classes` is provided, it will execute all test
code within those classes. Otherwise, it will execute all test code in all
classes.

The arguments can be auto completed.

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

Some keywords in SOQL can be auto completed, e.g. select, from, etc.

### dev execute [apex\_file]

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

### dev deploy [directory]

If `directory` is not provided, it will use `./src` as the directory.

Deploy the all the files in the directory to the Org. It is required that the
`package.xml` file is located in the directory.

### dev retrive [directory]

If `directory` is not provided, it will use `./src` as the directory.

Retrive all the files from the Org using the `package.xml` in the directory.
