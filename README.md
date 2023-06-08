# Podman-fedora-toolbx

Create a cattle container that uses a systemd unit template to create the latest fedora compatible custom toolbx.

## Modifications

1. This toolbx container **does not bind mount to the host home directory** like the toolbox create command does. Instead, it overlayers an filesystem on top of a skeleton directory located in  **${XDG_CONFIG_HOME}/skel**. If this directory doesn't exist, the service will clone one from **/usr/share/skel** directory into the location mentioned above. Modifications to the skeleton should be done before running this service because after the toolbx container is created you cannot modify it, thus creating a new container will cause you to lose your data. If you delete this container, you will lose your data.

2. I have added a bind mount to the host to share with this container. This service will create a shared directory on host at **~/Public/fedora-toolbx/project1** assuming you named the toolbx project1. This directory will be persistent even if you delete the toolbx container.

3. If you want to modify the service unit, remember to reload the daemon, as described below.

## Installation
1. Place the service unit file in **${XDG_CONFIG_HOME}/systemd/user** directory

	Reload systemd user manager configuration
	```sh
	systemctl --user daemon-reload
	```
## Running the service
2. If you wish to customize your skeleton directory, now is the time to do it. Running this unit template requires a name to call the toolbx container. The name you give it should be entered after the **@** when starting the unit. 

	The following command will create your named fedora toolbx
	```sh
	systemctl --user podman-fedora-toolbx@project1
	```
	Your new toolbox would be called **fedora-toolbx-project1**

3. You can verify the toolbx was created using the toolbox command
	```sh
	toolbox list
	```
4.  To enter the toolbx container
	```sh
	toolbox enter fedora-toolbx-project1
	```
##
2022 richiedaze
&nbsp;

SPDX-License-Identifier: LGPL-2.1-or-later
&nbsp;

fedora-toolbx unit template
