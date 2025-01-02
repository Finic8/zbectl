# zbectl

I wrote this tool some years ago when grub was the only bootloader that could boot a kernel directly form zfs.
However, ensuring that a pool has only features enabled that grub supports is cumbersome and this setup generally proved to be not as reliable as I wanted.
Nowdays using [zfsbootmenu](https://zfsbootmenu.org) offers a much better user experience in my opinion.
In conclusion I'm happy if this tool was useful to you in the past, but I'm not planing on maintaining it further.

## Functionality
zbectl is a utility for managing ZFS Boot Environments in Arch Linux similar to beadm on FreeBSD.

GRUB is used to boot the different environments.


## Architecture
* GRUB is used as the bootloader.
* zbectl __does not use the bootfs__ property. Instead it tells grub which entry to boot.
* Boot Environments are located under `poolname/ROOT/`.
For example: if your pool is named `zroot` then the environment called `default` is located under `zroot/ROOT/default`
### ESP mountpoints
* Each pool device has an [ESP](https://wiki.archlinux.org/index.php/EFI_system_partition)
* ESPs are automatically mounted under `/boot/efi/<partuuid>`

## Installation
- If you have never installed Arch before please read the [installation guide](https://wiki.archlinux.org/index.php/installation_guide) first, so you'll have a general idea of the requiered steps.
- Follow the instuctions on the Arch Wiki for creating an archiso with zfs support: <https://wiki.archlinux.org/index.php/Installing_Arch_Linux_on_ZFS>
- Include the package [zbectl-git](https://aur.archlinux.org/packages/zbectl-git/) from the aur or build it inside the livesystem.

### Partition layout
#### Automatic
Since version 2.0 zbectl can automatically partition your devices.
This works for EFI and BIOS systems. Make sure to boot your archiso in the same mode you want your installed system to boot! Simply run:

    zbectl partition <your device>...
For example if you want to use the two disks `sda` and `sdb` run:

    zbectl partition sda sdb

This will create the following layout on your devices.

BIOS
```
Part     Size   Type
----     ----   -------------------------
   1       2M   BIOS boot partition (ef02)
   2     XXXG   Solaris Root (bf00)
```
UEFI
```
Part     Size   Type
----     ----   -------------------------
   1     256M   EFI boot partition (ef00)
   2     XXXG   Solaris Root (bf00)
```
This will also create the requirred fat filesystem on your ESPs.

### Manual
If want to manually partition your devices please keep in mind that zbectl expects your ESP/BIOS partitions to be on the same parent device as you pool. The following steps assume you have already created a fat filesystem on each of your ESPs if using EFI.

### Installing your system
- Create your pool according to your desired layout over the partuuids returned to you by the `zbectl partition` command above. If you manually partitioned your devices you should know the right ids to use.

Please create your pool according to the guidelines on the [wiki](https://wiki.archlinux.org/index.php/ZFS#GRUB-compatible_pool_creation) to avoid grub not beeing able to boot from your pool.

Set the pool/dataset properties like ashift and compression according to your preference/use case.

- The only absolutely necessary datasets are the following:
```
zfs create <your_pool>/ROOT
zfs create -o mountpoint=/ <your_pool>/ROOT/<environment_name>
```
For example if your pool is named `zroot` and your environment `default` run:
```
zfs create zroot/ROOT
zfs create -o mountpoint=/ zroot/ROOT/default
```
- To avoid the pacman cache cluttering up your environments I recomend creating the following dataset:
```
zfs create -o mountpoint=/var/cache/pacman/pkg zroot/pkgcache
```
- Follow <https://wiki.archlinux.org/index.php/Installing_Arch_Linux_on_ZFS#Setup_the_ZFS_filesystem> until the bootloader secion.
- chroot into your new system and install [zbectl-git](https://aur.archlinux.org/packages/zbectl-git) from the aur.
- Run
```
zbectl install
```
- Continue with <https://wiki.archlinux.org/index.php/Installing_Arch_Linux_on_ZFS#Unmount_and_restart>.

## Usage
### zbectl list
Lists all ZFS Boot Environments.

### zbectl create [source] target
Creates a new ZFS Boot Environment named target. This clones the source environment. If no source environment is provided, the booted environment will be used.

### zbectl rename source target
Renames the source environment to target.

### zbectl destroy target...
Delets the target environment(s). Multiple targets can be specified.

### zbectl activate
Lists available environments.

### zbectl activate target
Sets the target as the default boot entry. Note that the entire name is not requirred, as long as the script can figure out which environment is ment.

### zbectl update
Updates grub.cfg. Run this after a GRUB update.

### zbectl mount target [mountpoint]
Mounts the target environment to /pool/ROOT/target or specified mountpoint.

### zbectl unmount target
Unmounts the target environment.

### zbectl chroot target [command]
Mounts the target environment to default mountpoint and chroots into it.
Optionally specify command to run inside the chroot. The target gets unmounted after the command exits.

### zbectl install [grub arguments]
Installs grub to all pool devices and and updates the grub.cfg. Additional arguments are passed to grub-install.

### zbectl partition [device]...
This will automatically partition your devices for EFI/BIOS booting.
Devices can be specified as absolute paths like `/dev/sda` or just the devicename like `sda`.

If your system is booted in EFI mode this will create a 256M ESP partition and a second partition for zfs on the specified devices.

In BIOS mode a BIOS boot partition for grub, and a second partition for zfs will be created.

## Troubleshooting
### GRUB
The GRUB package is unable to create boot entries for systems on ZFS. Zbectl uses an environment variable to resolve this.
For more information read the note on the Arch Wiki:
<https://wiki.archlinux.org/index.php/Installing_Arch_Linux_on_ZFS#error:_failed_to_get_canonical_path_of>

If you still receive an error you can try this older workaround:

The package includes a udev rule which creates symlinks. Run the following to activate it.

    udevadm trigger

## Exit Codes
The following exit codes are returned:

0   Successful execution.

1   An error occurred.

2   Invalid arguments or insufficient permissions.

3   A device related error occurred.
