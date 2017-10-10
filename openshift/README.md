# vscode-python
Developer environment based in vscode. After start point python environment to /opt/app-root


Run in docker:
```
docker run --name vscode -v /tmp/.X11-unix:/tmp/.X11-unix oondeo/vscode code`
```

Run in kube (socat needed)
```
socat EXEC:'kubectl run --image oondeo/vscode vscode -- socat unix-l\:/tmp/.X11-unix/X0 -' unix:/tmp/.X11-unix/X0

```
