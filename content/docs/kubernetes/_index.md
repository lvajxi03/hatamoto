+++
date = '2026-03-12T19:27:23+02:00'
title = 'Kubernetes'
menus = 'main'
type = 'page'
+++

This chapter contains varions information related to Kubernetes.

## Kubernetes

`k8s` is an open source container orchestration tool.

Helps you manage containerized applications, in a different environments.

### Uncategorized

#### ImagePullBackOff Error

This is a common error message in Kubernetes that occurs when a container running in a pod fails to pull the required image from a container registry.

#### Init containers

* Specialized containers that run before app containers in a pod
* init containers can contain utilities or setup scripts not present in app image
* do not support lifecycle, liveness probe, readiness probe or startup probe

#### Sidecar container

Container that starts before the main application container and _continues to run_.


#### Probe

* diagnostics performed periodically by the kubelet on a container
* it can be either execution of some code or a network request
* exec -- specified command
* grpc -- remote procedure call
* http get
* tcp socket

Result (outcomes): success, failure, unknown

##### Liveness probe

If fails, kubelet kills the container, and it is subjected to its restart policy/

##### Readiness probe

Indicates if container is ready to respond to requests.

If fails, endpoint controller removes pod's IPs adresses from the endpoints of all services that match the pod.

##### Startup probe

Indicates if app is started.

If fails, kubelet kills the container and it is the subject of its restart policy.

##### Restart policy

Possible values:

* Always -- restart after any termination
* On Failure -- only on error
* Never -- does not restart terminated container
