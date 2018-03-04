# zbectl

## Functionality
zbectl is a utility for managing ZFS Boot Environments in Arch Linux.

GRUB is used to boot the different environments.

## Requirements
* The EFI partition must be mounted under /boot/efi.
* GRUB has to be used as the bootloader.
* The script assumes that your Boot Environments are located under poolname/ROOT/.

## Installation
Simply install the package from the aur.
Then run: 

    zbectl install

It might be necessary to run this after a GRUB update as well.

## Usage
### zbectl list
Lists all ZFS Boot Environments.

### zbectl create [source] target
Creates a new ZFS Boot Environment named target. If no source environment is provided, the booted environment will be used.

### zbectl rename source target
Renames the source environment to target.

### zbectl destroy target
Delets the target environment.

### zbectl activate
Lists available environments.

### zbectl activate target
Sets the target as the default boot entry. Note that the entire name is not requirred, as long as the script can figure out which environment is ment.

### zbectl update
Updates grub.cfg.

### zbectl install [grub arguments]
Installs grub to /boot/efi and and updates the config. Additional arguments are passed to grub-install.

## Exit Codes
The following exit codes are returned:

0      Successful execution.

1      An error occurred.
