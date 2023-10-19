# Use Podman to run CommandBox with Lucee

This uses `podman` to run the ortussolutions/commandbox image and with the Lucee CFML engine. Podman may be installed in Linux or Windows Subsystem for Linux.

Project structure:

```text
.
├── app
    └── .cfconfig.json
```

## Pull Commandbox image

```bash
podman pull docker.io/ortussolutions/commandbox
```

## Run Commandbox image in daemon mode

This starts Commandbox with lucee 6.0.1 engine in daemon mode `-d`. TCP port 8080 in the container is mapped to port 8080 on the host.

```bash
$ podman run -d -p 8080:8080 -e "BOX_SERVER_APP_CFENGINE=lucee@6.0.1" ortussolutions/commandbox
ad308b2369046c0f2245699243c2094ebfc800b5ae0ede8038e0faa4c8aeb605
$ 
```

### Default files included with Commandbox

Unless a volume mapping is added for /app, the Commandbox Lucee image defaults to providing a few files in that location: index.cfm (a welcome page), 403.html, and CommandBoxLogo300.png.

In that case, you could run `http://localhost:8080/index.cfm` in your web browser to see the test page, or run via curl:

```bash
curl http://localhost:8080/index.cfm
```

### Enabling Lucee Admin and Setting admin password

As with Lucee itself, for the sake of security, the admin pages are unavailable and there is no password defined for the Lucee admin(s) in Commandbox by default. To enable Lucee Admin:

1. The environment variable `BOX_SERVER_PROFILE` must be set to `development`
2. The ./app folder in this repo is mapped to /app in the container
3. The .cfconfig.json file in the ./app folder contains the desired `adminPassword` value

```bash
podman run -d -p 8080:8080 -v "./app:/app" -e "BOX_SERVER_PROFILE=development" -e "BOX_SERVER_APP_CFENGINE=lucee@6.0.1" ortussolutions/commandbox
```

### Accessing Lucee Admin

The Lucee admin (server and web) are available within the lucee/lucee container via the paths lucee/admin/server.cfm and lucee/admin/web.cfm, respectively.

If enabled, to access the Lucee Server admin, navigate to `http://localhost:8080/lucee/admin/server.cfm` in your web browser or run:

```bash
curl http://localhost:8080/lucee/admin/server.cfm
```

## Helpful Podman Commands

### List running containers

```bash
$ podman ps
CONTAINER ID  IMAGE                                       COMMAND               CREATED         STATUS             PORTS                   NAMES
0a22d49ddf34  docker.io/ortussolutions/commandbox:latest  /bin/sh -c $BUILD...  11 seconds ago  Up 11 seconds ago  0.0.0.0:8080->8080/tcp  loving_cerf
```

### View container logs

```bash
podman logs loving_cerf
```

### Stop container

```bash
podman stop loving_cerf
```

## Troubleshooting

In WSL2, after stopping the container and attempting to start again with `podman run`, it fails with `bind: address already in use`. This is likely due to port 8080 being used by another process. Identify the process and process id with `sudo lsof -i -P -n | grep -E '8080|PID'` then kill the offending process with `kill <pid>`.

For a more permanent solution, install dbus-user-session exit/shutdown wsl/go back into wsl.

```bash
$ sudo apt install dbus-user-session
$ exit

> wsl --shutdown
```
