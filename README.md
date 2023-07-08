# Automount non-EXT4 SD cards on your steam deck

The steam deck uses a shell script in order to mount SD cards automatically on startup as well as when hotswapped. However, this script only automatically
mounts EXT4 formatted SD cards, however, this can be changed very easily. Heres a short demo of how to modify the script for NTFS and BTRFS formatted 
SD cards.

## The Script

File is originally located at
```bash
/usr/lib/hwsupport/sdcard-mount.sh
```
on your steam deck. Open konsole and set a password for your user if you havent already with this command:
```bash
passwd
```
then, you can edit the file with the nano terminal editor using:
```bash
sudo nano /usr/lib/hwsupport/sdcard-mount.sh
```

## NTFS
Change these lines from:
```bash
# We need symlinks for Steam for now, so only automount ext4 as that'll Steam will format right now
    if [[ ${ID_FS_TYPE} != "ext4" ]]; then
       echo "Error mounting ${DEVICE}: wrong fstype: ${ID_FS_TYPE} - ${dev_json}"
       exit 2
    fi
```
to:
```bash
# We need symlinks for Steam for now, so only automount ext4 as that'll Steam will format right now
    # if [[ ${ID_FS_TYPE} != "ext4" ]]; then
    #    echo "Error mounting ${DEVICE}: wrong fstype: ${ID_FS_TYPE} - ${dev_json}"
    #    exit 2
    # fi
```

## BTRFS
Change the same lines as mentioned in NTFS, and I would also recommend changing the following
lines in order to enable compression:

from:
```bash
#if [[ ${ID_FS_TYPE} == "vfat" ]]; then
    #    OPTS+=",users,gid=100,umask=000,shortname=mixed,utf8=1,flush"
    #fi
```
to:
```bash
if [[ ${ID_FS_TYPE} == "btrfs" ]]; then
        OPTS+=",compress-force=zstd:3,autodefrag"
    fi
```
Using compression will save you a ton of space on your SD cards depending on the game,
without performance loss.(Possibly even performance gains in some cases)

## Conclusion
After those lines have been modified, just save the script, and restart your steamdeck,
and the SD card should automatically mount.

Example modified versions of the scripts for [BTRFS](https://github.com/justinlime/sdcard-mount.sh/blob/master/sdcard-mount.btrfs.sh) and [NTFS](https://github.com/justinlime/sdcard-mount.sh/blob/master/sdcard-mount.ntfs.sh) can be found within this repo
