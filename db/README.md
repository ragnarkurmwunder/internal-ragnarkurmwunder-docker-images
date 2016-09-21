# Mysql Database

## Prerequisites

* Docker installed
* GNU make installed
* Pwgen installed
* Root access - Needed to change file ownerships, because container uses same numeric id-s as host.
* Mysql image - The image has to be present, use `docker pull mysql`

## Workflow (`make`)

Whole workflow here is governed by a Makefile.

Currently following make targets are available:

* start - (stop and) start the container
* stop - stop the container
* cli - mysql command line interface
* cli\_notty - execute sql from command line
* perms - reset mysql file and folder permissions
* build - (clean and) init mysql database, add a user
* clean - remove mysql files and dirs, remove generated data

You can aquire this list just by typing `make`.

### Initializing (`make build`)

Before first usage of the mysql, you need to create/init the database.

* removes mysql files and dirs, removes generated data
* generates new credentials
* creates necessary files and folders for mysql
* resets mysql file and folder permissions
* builds initial database
* adds one user and one database
* prints out database access information (database link name, database name, user and password)

### Start (`make start`)

To start the database.

* stops and removes the container
* starts the container

This command can be used to start and restart the container, both.

### Stop (`make stop`)

To stop the database.

* stops the container, if it runs
* removes the container, if it exists

### Command Line Interface (`make cli` and `make cli_notty`)

Access mysql from commandline.
You will be dropped to mysql shell.

If you need to run sql directly from commandline or from script,
then you need to use `make cli_notty`.

```
echo "select 1;" | make sql_notty
```

### Cleanup

Cleanup project if anything goes wrong with it.

* removes mysql files and folders
* removes generated data

### Permissions

Reset owner, group, permissions.

* assigns right ownership to mysql files and folders

## Files and folders

### Mysql stuff

#### my.cnf

This is mysql configuration file which is going to be mapped into container,
so you could modify it according to your needs. Current customisation is logging location.
If you need to check slow log, turn it on here.

#### data/

This is folder where all mysql db files live.

#### logs/

This folder contains mysql logs, which can be observed from host.
For example:

```
$ tail -f logs/slow.log
```

### Workflow stuff

#### Makefile

This is GNU make Makefile, which contains all the rules for smooth workflow.
See Workflow section.

#### config.mk

This is configuration for Makefile, where it is specified all file and folder locations to be used.
In standard project there is no need to change this.

#### waitbuild

This is expect script needed to detect when mysql has finished initializing the database.
