
mkfs.ubifs without compression

```shell
mkfs.ubifs -m 1 -e 65408 -c 1024 -r images.tree -o images.ubifs -x none -F
```

generating ubi image

```shell
ubinize -o images.ubi -m 1 -p 65536 images.ubinize.cfg
```

ubinize.cfg example

```toml
[images_volume]
mode=ubi
image=images.ubifs
vol_id=0
vol_type=dynamic
vol_name=images
vol_aligment=1
vol_flags=autoresize
```
