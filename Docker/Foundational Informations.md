## Basics
- docker is used to automate the deployment of software apps inside container

## Container
- provides layer of abstraction and automation of OS-level virtualization on Linux
	- logical packaging mechanism where apps can be abstracted from the environment in which they actually run
		- allows container-based apps to be easily + consistently deployed
			- Due to predictable environments isolated from rest of apps -> granular control

- created from docker images and run the actual apps
	- created using docker run

- run on the host operating system
	- allows user to package an application with all of its dependencies into a standardized unit for software development
- Leverage low-level mechanics of the host operating system
	- provide most of the isolation of VMs

- Benifits
	- doesn't have high overhead
	- enable more efficient usage of the system and resources
	- Do not run a guest operating system
		- these run on virtual hardware powered by the server's host OS
			- great for isolation between host and guest OS
			- though great costs to virtualize hardware for guest OS

## Images
- blueprints of our application which form the basis of containers

## Docker Daemon
- Background service running on the host that manages building, running and distributing Docker containers
- Process that runs in the OS which clients talk to

## Docker Client
- Command line tool that allow user to interact with the daemon

## Docker Hub
- registry of docker images
- one can host own docker registries and can use them for pulling images 