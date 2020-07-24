# snake-runner helm chart

This repository contains a Helm chart to run snake-runner on Kubernetes.

The native support for Kubernetes [is in progress](https://trello.com/c/JSYWFXyP/30-kubernetes-native-support),
this implementation provides two alternative ways to integrate snake-runner into your Kubernetes
cluster.

### Mounting host's docker.sock

This is a very similar approach to what we wrote about in our technical documentation:
[Using Docker in CI: Docker Socket Binding](https://snake-ci.com/docs/tutorials/using-docker-in-ci/#docker-socket-binding)

Pass `snake.docker_mode=host` as additional value to Helm chart while installing it.

### Docker-in-Docker

Unfortunately, this method requried the Privileged mode.

Read more about DIND in Docker's blog:
[Docker now can run within Docker](https://www.docker.com/blog/docker-can-now-run-within-docker/)

Pass `snake.docker_mode=dind` as additional value to Helm chart while installing it.

### Kubernetes Native Support

The task is in progress, but it's one of our highest priorities. Native support means that Snake
Runner will use Kubernetes API to create Pods instead of creating Docker containers directly on
hosts.

## Installation

Create your own values.yaml file to override default values, make sure to specify values in `snake`
directive:
Key                        | Attribute | Description
:-                         | :-        | :-
`snake.master_address`     | Required  | The address of Bitbucket with Snake CI Add-on installed.
`snake.registration_token` | Required  | The security token for registering the runner. You can obtain it in the Bitbucket admin panel → Runners.
`snake.docker_mode`        | Required  | "host" or "dind". The option to mount docker.sock into snake-runner's container

Checkout [values.yaml](values.yaml) for more values.

Add the Reconquest Helm repository:
```
helm repo add reconquest https://helm.reconquest.io
```

For Helm 2:
```
helm install --namespace <NAMESPACE> --name ci -f <VALUES_FILE> reconquest/snake-runner
```

For Helm 3
```
helm install --namespace <NAMESPACE> ci -f <VALUES_FILE> reconquest/snake-runner
```
