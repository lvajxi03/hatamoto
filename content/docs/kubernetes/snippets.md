+++
date = '2026-03-12T19:49:23+02:00'
title = 'Snippets'
menus = 'main'
type = 'page'
+++

## Various snippets

```sh
kubectl get nodes | cut -f1 -d '' | xargs -I{} kubectl get node/{} -o yaml | grep address
```

```sh
for i in `kubectl get pods | aws '{print $2}'`; do echo -n `kubectl get pod/{i} -o yaml | grep -i hostip` && echo " -> $i "; done | sort
```
