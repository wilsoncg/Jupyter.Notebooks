[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/wilsoncg/Jupyter.Notebooks/net5?urlpath=%2Fnotebooks%2FXPlot.ipynb)

# .NET Interactive Docker image

This folder contains a Dockerfile that generates an image with the latest .NET Interactive and Jupyter. This lets you try out .NET Interactive's Jupyter experience without needing to install Jupyter directly.

## Build instructions

You can build the Docker image by running the following command in this directory:

```powershell
> docker build . --tag dotnet-interactive:1.0
```

The container exposes port 8888 and the port range 1100-1200. A different port range can be specified at build time.

Port 8888 is used by Jupyter and it is where the web application runs, the range is used by dotnet interactive to expose functionalities to the frontend directly.

```powershell
> docker build . --tag dotnet-interactive:1.0 --build-arg HTTP_PORT_RANGE=1000-1100
```

## Run the container

To run it execute the following command:

```powershell
> docker run --rm -it -p 8888:8888 -p 1100-1200:1100-1200  --name dotnet-interactive-image dotnet-interactive:1.0
```

Remember to match the port range with the range used when building the image.

A JupyterLab instance will be published on `http://127.0.0.1:8888/`. Check the output of the `docker run` command for the full URL which includes the Jupyter authentication token.

## Persistent storage and local notebooks

Local folder can be mounted using the `--mount option`, follow the instruction to expose your host drive to Docker.

On Windows the following command will mount a local folder into the running image at the `/notebooks` mountpoint

```powershell
docker run --rm -it -p 8888:8888 -p 1100-1200:1100-1200 --mount=type=bind,src="$(pwd)/notebooks",target=/home/jovyan/notebooks --name dotnet-interactive-image dotnet-interactive:1.0
```

# Errors
## Port not available
```
C:\Program Files\Docker\Docker\resources\bin\docker.exe: Error response from daemon: Ports are not available: listen tcp 0.0.0.0:1112: bind: An attempt was made to access a socket in a way forbidden by its access permissions.
time="2020-07-22T16:53:01+01:00" level=error msg="error waiting for container: context canceled"
```
```
netsh int ipv4 show excludedportrange protocol=tcp

Protocol tcp Port Exclusion Ranges

Start Port    End Port
----------    --------
      1036        1135
```

### References
* `Readme.md` from https://github.com/dotnet/interactive/blob/master/samples/docker-image/readme.md