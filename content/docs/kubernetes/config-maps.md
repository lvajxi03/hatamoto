+++
date = '2026-03-12T19:47:23+02:00'
title = 'Config Maps'
menus = 'main'
type = 'page'
+++

## Config Maps

API objects used to store non-confidential data in key-value pairs.

Pods can consume Config Maps as environment variables, command line arguments or as configuration files in a volume.

Use a Config Map for setting configuration data separately from application code.

```sh
kubectl create cm demo-cm --from-file data.txt
```
