# artix-luks-base-install

A fully self-contained Artix Linux base installation script that installs the
base system with an encrypted disk on /root /home and /swap using cryptsetup.
The script is fully automated with no interaction from start to finish.
Parameters are given as command line arguments.

The installer is only configured to work with EFI systems and installs the runit
init system by default. By default it allocates 12G for /swap, 50G for /root and
everything else for /home. This should be sufficient for most configurations.

Features:
- EFI only.
- Encrypted /root /home and /swap using LVM on LUKS encryption technique.
- Runit init system (replaces systemd).
- Configures defaults for locale, sudoers and networking.
- Small and well documented script, no bloat.

The following articles we're used for reference:
- [https://wiki.artixlinux.org/Main/Installation](https://wiki.artixlinux.org/Main/Installation)
- [https://wiki.artixlinux.org/Main/InstallationWithFullDiskEncryption](https://wiki.artixlinux.org/Main/InstallationWithFullDiskEncryption)
- [https://wiki.archlinux.org/title/installation_guide](https://wiki.archlinux.org/title/installation_guide)
- [https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS)


# Download.
You can download this script raw from github.
```console
curl -LO https://raw.githubusercontent.com/swordstrike1/artix-luks-base-install/master/artix-luks-base-install
```


# Usage.
Help is echoed by the script.
```console
$ ./artix-luks-base-install /dev/sdX [ENCRYPTION-PASSWORD]
```


# Wifi Setup With Connmanctl.
To use this script, make sure you are connected to the internet. You can connect
to the internet either through ethernet or with connmanctl:
```console
connmanctl
> agent on
> enable wifi
> scan wifi
...
Scan Completed
> services
... A long list of services SSIDS & codes ...

> connect <NETWORK ID>
Enter Password: ******
...
Connected
> exit
```


# Install Summary.
The script follows the following install steps:

1. Ask user confirmation.
1. Download parted using pacman.
1. Reset previous installation attempts.
1. Partition disk using parted. (512MB boot, 100% leftover to encrypted drive)
1. Create LUKS container using cryptsetup.
1. Create physical volume.
1. Create logic volume group (default name CryptVolGroup).
1. Create logical volume SWAP size 12g.
1. Create logical volume ROOT size 50g.
1. Create logical volume HOME leftovers.
1. Make filesystems.
1. Mount filesystems.
1. Basestrap system and generate fstab.
1. Modify grub cfg and install grub with UEFI.
1. Set root password to 123.
1. Modify sudoers to allow 'wheel' group sudo access with root password only and set pwfeedback.
1. Set hostname (default 4rt1x).
1. Create /etc/hosts file.
1. Link NetworkManager service to runsvdir.
1. Generate and set default locale to en_US.UTF-8.
1. Set default timezone to Europe and sync hwclock.
1. Perform cleanups.
1. Inform user :)

If you have suggestions on improving the script, or need help with the
installation. Feel free to contribute/open issues.
