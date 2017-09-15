# nignx-bg

Nignx Docker container for demonstrating blue/green and rolling reployments. 

A static web page which loops a simple JSON request returning a context of either blue or green. The imbedded javascript continuously requests the backend document there is no need to refresh the webpage to demonstrate state changes.

### Build and Run Container
```bash
$> docker build -t nginx-bg .
...
$> docker run -p 8000:80 nginx-bg
...

```

### Docker Hub

The "latest" tag is the same as blue.
And the "green" tag is well... green!

Docker Repo : [cuzz22000/nginx-bg/](https://hub.docker.com/r/cuzz22000/nginx-bg/)

```
$> docker run -p 8000:80 cuzz22000/nginx-bg:latest
```



