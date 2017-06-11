# zbectl

## Functionality
zbectl is a utility for managing ZFS Boot Environments in Arch Linux.

It creates a folder with the name of the Boot Environment in /boot in which the kernels are stored.

Because mkinitcpio needs the kernel to	be in /boot, the package includes alpm-hooks to move the kernels accordingly.

Boot entries for systemd-boot  are also created automatically.

The script assumes that your Boot Environments are located under poolname/ROOT/

## Requirements
At the moment you have to be using systemd-boot.

The script assumes that your Boot Environments are located under poolname/ROOT/.

## Installation
Simply install the package from the aur.
Then run 

    zbectl upgrade-kernel

## Usage
### zbectl list
Lists all ZFS Boot Environments.

### zbectl create [source] target
Creates a new ZFS Boot Environment named target. The dataset and the kernels are copied from the running environment unless different source environment is specified. Bootentries are created accordingly.

### zbectl rename source target
Renames the source environment  to  target. Boot entries get renamed automatically.

### zbectl activate target [kernel]
Sets the target as the default boot entry. Kernel must be specifed if more than one is available.

### zbectl destroy target
Delets the target environment.

### zbectl preupdate
Copies the kernels of the running boot environment to /boot. This is necessary because mkinitcpio expects them to be there. Gets run be the preupdate hook.

### zbectl kernel-upgrade
Moves new/upgraded kernels back to /boot/target and creates boot entries for newly installed kernels. Gets run by the install/upgrade hook.

### zbectl kernel-remove
Moves  kernels  back to /boot/target and delets boot entries for no longer installed kernels. Gets run by the remove hook.

## Exit Codes
The following exit codes are returned:

0      Successful execution.

1      An error occurred.

