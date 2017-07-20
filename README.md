# zbectl

## Functionality
zbectl is a utility for managing ZFS Boot Environments in Arch Linux.

GRUB is used to boot the different environments.

## Requirements
* The EFI partition must be mounted under /boot/efi.
* GRUB has to be used as the bootloader.
* The script assumes that your Boot Environments are located under poolname/ROOT/.


__To Be Noted__

The GRUB package is unable to boot from ZFS as described on the Arch Wiki:

<https://wiki.archlinux.org/index.php/Installing_Arch_Linux_on_ZFS#error:_failed_to_get_canonical_path_of>

To avoid recompiling GRUB, this error gets resolved by a udev rule which creates a symlink.

## Installation
Simply install the package from the aur.
Then run: 

    zbectl install

It might be necessary to run this after a GRUB update as well.

## Usage
### zbectl list
Lists all ZFS Boot Environments.

### zbectl create [source] target
Creates a new ZFS Boot Environment named target. The dataset and the kernels are copied from the running environment unless different source environment is specified. Bootentries are created accordingly.

### zbectl rename source target
Renames the source environment  to  target. Boot entries get renamed automatically.

### zbectl destroy target
Delets the target environment.

### zbectl activate
Lists available environments.

### zbectl activate target
Sets the target as the default boot entry.

### zbectl install [grub arguments]
Installs grub to /boot/efi and and updates the config. Additional arguments are passed to grub-install.

## Exit Codes
The following exit codes are returned:

0      Successful execution.

1      An error occurred.
