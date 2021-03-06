Deploy Script
===================

The deploy script is a tool to easily deploy new releases of your webbased applications.
It's goal is to grab the new source code from your versioning system (Git or Subversion) and safely deploy it to a (production) environment.
Functionality included are things like:

- Perform a database backup prior to deployment
- Performa a file backup prior to deployment
- Send notifications via e-mail or Pushover after deployment
- Hot swap to new code base, so there's virtually no downtime


Available subcommands are:
----

  		--config <name>		Will use configuration file deploy.<name>.conf.php. default file: deploy.conf.php.
  		--tag <tag #>		Git/SVN tag to be deployed.
  		--branch <branch>	Git/SVN branch to be deployed.
  		--debug				Debug mode: default = false.
  		--quiet				Quiet mode, only output warnings and exceptions, only if debug is not given: default = false.
  		--version			Shows version information
  		-c <name>			Alias for --config.
  		-t <tag #>			Alias for --tag.
  		-b <branch>			Alias for --branch.
  		-d					Alias for --debug.
  		-q					Alias for --quiet.

Installation
----
- Grab project from GitHub
- Copy the example file for your project from sources/.. to the root of the project
- Fill in the configuration where still blank


###Installation Concrete 5
Log into the server and grab a copy of the script.

	$ clone git://github.com/nedstars/Deploy-Script.git

Copy the example file for each enviroment that your need, for example "staging" or "live", from sources/.. to the root of the project.
When executing ./deploy the -c or --config argument is used to specify which config should be loaded.
When no explicit configuration is given the default one is used (deploy.conf.xml)

	$ cp sources/example.concrete5.conf.xml <config_name>.conf.xml

Fill in the <config_name>.conf.xml file where still blank.

- Database connection data or use local.xml loader.
- Git repo, deploy script uses git --archive to get the source code from the repo for your deployment
- Notifications, configure how to notify peopele when deployemnt is done.
- Paths, there are 4 folders that need to be configured
	- live path, path to project folder (the one that will be updated)
	- temp new path, this is the folder where the new source will be build up.
	- temp old path, when switching to the new source the old source is copied to this folder
	- backup path, folder where all backups will be played

Execute example

	$ ./deploy -c <config> -t <tag number>

Notifications
----
There are two types of notification services:

- E-mail
- Pushover (https://pushover.net/)

The XML looks like:

	<notifications>
		<email_addresses>
			<address>example@example.com</address>
		</email_addresses>
		<pushover_users>
			<user>183SSd882exampleS82</user>
		</pushover_users>
	</notifications>


Hooks
----
There are 5 hook groups

- Data
- Backup
- Notifications
- Source
- Deployer

the classname and file name should be the same if you want to implement a hook
classname: Hooks_{{group}}Interface
filename: Hooks_{{group}}Interface.php

The XML looks like

	<hooks>
		<files>
			<file>Hooks_DataInterface.php</file>
			<file>Hooks_SourceInterface.php</file>
		</files>
	</hooks>
