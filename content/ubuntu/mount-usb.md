# Mount an NTFS USB drive as current user on Ubuntu

To mount an NTFS USB drive with read/write permissions for your current user and open it in the file manager.

## 1. Identify the USB partition

```bash
lsblk -f
```

Find your USB drive by its size, label and filesystem.

Example:

```text
sdb
└─sdb1 ntfs Usb Storage
```

In this example, the partition is `/dev/sdb1`.

**Important:** Your USB device name might be different, such as `/dev/sdc1`. Always check before running mount commands.

## 2. Check current user and group IDs

```bash
id
```

Example:

```text
uid=1000(atari-monk) gid=1000(atari-monk) groups=...
```

You can use `$(id -u)` and `$(id -g)` directly in the mount command to automatically obtain these values.

## 3. Mount the USB drive

```bash
sudo umount /mnt/usb 2>/dev/null
sudo mkdir -p /mnt/usb
sudo mount -t ntfs3 -o rw,force,uid=$(id -u),gid=$(id -g),umask=022 /dev/sdb1 /mnt/usb
```

Replace `/dev/sdb1` with your actual USB partition.

## 4. Open in file manager

```bash
xdg-open /mnt/usb
```

You should now be able to copy, create, delete and modify files.

## 5. Safely disconnect

Before physically unplugging the USB drive:

```bash
sync
sudo umount /mnt/usb
```

## Command reference

| Command/option | Purpose                             |
| -------------- | ----------------------------------- |
| `lsblk -f`     | Identify disks and partitions       |
| `id`           | Check current user and group IDs    |
| `uid=$(id -u)` | Assign ownership to current user    |
| `gid=$(id -g)` | Assign current user's group         |
| `rw`           | Enable read/write access            |
| `force`        | Bypass NTFS dirty-volume protection |
| `umask=022`    | Set default permission mask         |
| `xdg-open`     | Open directory in file manager      |
| `sync`         | Flush pending writes                |
| `umount`       | Safely unmount drive                |

**Important:** The `force` option bypasses NTFS dirty-volume protection and carries a risk of filesystem corruption. Back up important data and use Windows `chkdsk /f` when possible. Replace device names with the correct values identified on your system.
