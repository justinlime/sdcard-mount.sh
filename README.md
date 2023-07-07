# Automount non-ext4 SD cards on your steam deck
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
        OPTS+=",compress-force=zstd:3"
    fi
```
Using compression will save you a ton of space on your SD cards depending on the game,
without performance loss.(Possibly even performance gains in some cases)

## Conclusion
After those lines have been modified, you can just restart your steamdeck,
and the SD card should automatically mount.