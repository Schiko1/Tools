- [] -> optional

- <span style="background:#ff4d4f">docker pull</span> "image_name"
	- fetches this image from Docker registry and saves it to the system

- <span style="background:#ff4d4f">docker images</span>
	- list of all images on the system

- <span style="background:#ff4d4f">docker run</span> "image-name"
	- docker client finds image, loads up the container and runs a command in that container
		- if no command is provided, then container boots up, run empty command and then exits
	- **Additional inputs:**
		- [linux command] : after the image name, like echo
		-  <span style="background:#fff88f">-it</span> : attaches you to an a interactive tty in the container
			- -i : interactive
			- -t:  pseudo-TTY
			- add also **<span style="background:#b1ffff">sh</span>** or just <span style="background:#b1ffff">bash</span> to the end of command
		- <span style="background:#fff88f">-d</span> : run in detached mode(in background)
		- <span style="background:#fff88f">-- name</span> "container_name" "image": gives container name
		- <span style="background:#fff88f">-p</span> "host_port":"container_port" : map container's port to host
		- <span style="background:#fff88f">-v</span> "host_path":"container_path"
			- Volume: Mount host directory/file inside container
		- <span style="background:#fff88f">-- mount</span> type=bind, source="host", target="container"
			- More explicit, modern syntax
			- docker run -v $(pwd)/data:app/data "myimage"
		- <span style="background:#fff88f">-e</span> KEY=VALUE
			- SET environment variable
		- <span style="background:#fff88f">--env-file</span> "file"
			- Load multiple env vars from a file
		- <span style="background:#fff88f">--restart</span> always
			- Restart container automatically
			- also: no/unless-stopped/on-failure
		- <span style="background:#fff88f">-m</span> "memory"
			- limit memory (500m,2g)
		- <span style="background:#fff88f">--cpus</span> "num"
			- LImit CPU usage
		- <span style="background:#fff88f">--gpus</span> all
			- if nvidia runtime -> Use GPU
		- <span style="background:#fff88f">-w</span> "path"
			- Set working directory inside the container
		- <span style="background:#fff88f">--rm</span>
			- Automatically remove container when it exits

- <span style="background:#ff4d4f">docker ps</span>
	- shows all containers that are currently running
	- **Additional inputs**:
		- -a : shows all container that were run

- <span style="background:#ff4d4f">docker rm</span> "container_ID"
	- removes container
	- docker rm $(docker ps -a -q -f status=exited)
		- deletes all containers with status exited
		- -q returns numeric IDs only and -f is filter
	- also works docker container prune

- <span style="background:#ff4d4f">docker rmi</span>
	- remove image that are no longer needed