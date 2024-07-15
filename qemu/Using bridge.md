
```bash
qemu-system-x86_64  -device virtio-net-pci,netdev=hn0,id=nic1 -netdev bridge,id=hn0,br=virbr0
```

