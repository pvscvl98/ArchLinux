# Backups on Linux

## [rsync/grsync]()
<!--
[video rsync](https://www.youtube.com/watch?v=oS5uH0mzMTg)

[video grsync](https://www.youtube.com/watch?v=zQ6yba3Sl7k)
-->
USB device filesystem which is used for storing the backup should have an ext4 filesystem because of compatibility issues. A FAT filesystem wont be able to store some ext4 files which could lead to the backup being corrupted.

It is recommended to initially _--dry-run_ the backup in order to catch any mistakes.

backup command
```
sudo rsync -aAXv --delete --exclude=/dev/* --exclude=/proc/* --exclude=/sys/* --exclude=/tmp/* --exclude=/run/* --exclude=/mnt/* --exclude=/media/* --exclude="swapfile" --exclude="lost+found" --exclude=".cache" --exclude="Downloads" /source /destination
```
source = /  
destination = mount point of usb ( /mnt/... )

restore command
```
sudo rsync -aAXv --delete --exclude="lost+found" /backup /system
```
```
-a
-A
-X
-v
--delete
--dry-run
```

---

## [tar]()

backup
```
sudo tar -cvpzf filename.tar.gz --exclude=/mnt /
```
```
-c      create
-v      verbose
-p      preserve permissions
-z      compression
-f      filename
```

restore
```

```

