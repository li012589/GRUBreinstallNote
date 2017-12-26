# grub reinstall

1. Check disk partitions by run `sudo fdisk -l`

2. Mount all your file system

   ```bash
   sudo mount /dev/sda1 /mnt
   sudo mount --bind /dev /mnt/dev
   sudo mount --bind /proc /mnt/proc
   ```

3.  Chroot to /mnt by `sudo chroot /mnt`.

4. Run `grub-install /dev/sda`.

5. Do some clean-ups.

   ```bash
   exit
   sudo umount /mnt/dev
   sudo umount /mnt/proc
   sudo umount /mnt
   sudo reboot 
   ```

## optional: windows grub configure

To disable os-prober add the following line to `/etc/default/grub`:

```bash
GRUB_DISABLE_OS_PROBER=true
```

Discover the UUID of the partition containing the Windows boot loader by executing the following command in terminal as root:

```bash
blkid /dev/sda1
```

The output will be something like this:

```bash
/dev/sda1: LABEL="System Reserved" UUID="1D584C40586B2873" TYPE="ntfs" PARTUUID="adc19fb9-33"
```

Use the UUID value to create a custom menu entry at the end of the `/etc/grub.d/40_custom` file:

```bash
menuentry "Windows 10" --class windows --class os {
   insmod ntfs
   search --no-floppy --set=root --fs-uuid 1D584C40586B2873
   ntldr /bootmgr
}
```

Update grub by executing the following command in terminal as root:

```bash
update-grub
```

