
## Config from buildroot-2017.11.1

```txt
freescale_mpc8315erdb_defconfig
```

## Buildroot version buildroot-2024.02.3

```bash
wget https://www.buildroot.org/downloads/buildroot-2024.02.3.tar.xz
```

## Kernel version (linux-4.5.3)

```bash
wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.5.3.tar.xz
```

## Move board/freescale/mpc8315erdb/* to buildroot-2024.02.3 (linux-4.5.3)

```bash
cp -rv board/freescale/mpc8315erdb /path/to/buildroot-2024.02.3/board/freescale/
```

## U-boot config

```txt
cpu8315e_nand_config
MPC8315ERDB_defconfig
```

