+++
date = '2026-03-13T15:15:23+02:00'
title = 'GRUB'
menus = 'main'
type = 'page'
+++

## GRUB

`/etc/default/grub`:

```
...
GRUB_DEFAULT=0
...
```

and then:

```sh
update-grub
```
