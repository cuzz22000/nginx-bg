# nignx-ng

Nignx Docker container for demonstrating blue/green and rolling reployments. 

A static web page which loops a simple JSON request returning a context of either blue or green. Since the javascript imbedded in the page continually requests backend the service there is no need to refresh page to demonstrate state changes.

### Build and Run Container
```bash
$> docker build -t nginx-bg .
...
$> docker run -p 8000:80 nginx-bg
...

```


