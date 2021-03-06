# Problem with VsCode Remote Containers and Host env Vars

VsCode is interferring with environment variables I set with my docker-compose files.

- it works with docker run
- it works with plain `docker-compose run`
- it doesn't work with "Reopen in container"

1. clone
2. open in VsCode
3. have Remote Containers: Docker
4. open terminal configured to use Powershell, then run:
```
$env:VSCODE_ENV_VAR_TEST='foo'
```
5. run the following first:
```
docker-compose -f docker-compose.yml -f docker-compose--local.yml run the-service env | grep VSCODE_ENV_VAR_TEST

# expected:
# VSCODE_ENV_VAR_TEST='foo'
```
6. just to be sure, run: (docker-compose seems to normalise the variance in windows ENV vars)
```
docker run --rm -it -e VSCODE_ENV_VAR_TEST=${env:VSCODE_ENV_VAR_TEST} microsoft/vscode-bug-with-remote-containers-and-environment-variables env | grep VSCODE_ENV_VAR_TEST

# expected:
# VSCODE_ENV_VAR_TEST='foo'
```
7. then open in container: ctrl+shift+p "Remote-Containers: Reopen in container"
8. now run:
```
env | grep VSCODE_ENV_VAR_TEST

# expected:
# VSCODE_ENV_VAR_TEST='foo'
# actual:
# (blank)
```