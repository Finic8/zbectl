# zbectl

## Functionality
zbectl is a utility for managing ZFS Boot Environments in Arch Linux.

GRUB is used to boot the different environments.

zbectl does not use the __bootfs__ property. Instead it tells grub which entry to boot.

## Requirements
* If you use EFI, the ESP partition must be mounted under `/boot/efi`.
* GRUB has to be used as the bootloader.
* Boot Environments are located under `poolname/ROOT/`.

## Installation
Install the package `zbectl-git` from the aur: <https://aur.archlinux.org/packages/zbectl-git/>

### EFI
Make sure your ESP partition is mounted under /boot/efi. Then run:

    zbectl install

### Legacy
Zbectl will automatically switch to BIOS booting if no efi partition is mounted:

    zbectl install /dev/sda

Where `/dev/sda` is the disk GRUB will be installed on.

For additional information read:
<https://wiki.archlinux.org/index.php/GRUB#BIOS_systems>

### Manual
If your manually install GRUB, run:

    zbectl update

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

### zbectl install [grub arguments]
For EFI booting only. Installs grub to /boot/efi and and updates the config. Additional arguments are passed to grub-install.

### zbectl install /dev/sdx [grub arguments]
For BIOS booting. Installs grub to /dev/sdx and and updates the config. Additional arguments are passed to grub-install.

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

0      Successful execution.

1      An error occurred.
