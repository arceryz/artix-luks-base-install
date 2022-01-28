# artix-luks-base-install

Features:
- LVM on LUKS encrypted drive (see below Arch Linux article).
- Artix runit init system only (can be changed).
- Partitioning: 12g swap, 50g root, leftover to home. 512MB boot. (can be changed).
- UEFI only (can be changed).
- Allow wheel group in sudoers with root password only.
- Set locale and other essential defaults.
- Networkmanager setup and linking.
- Bare minimum for bootable system. Make personal after installing base.
- Log in with root password '123'.
- Small and well documented script. No bloat.

Artix Linux LUKS encrypted minimal automated installation script. Download &amp; Run.
This script will install a bare minimal encrypted artix installation from the base
iso. The script loosely follows the following articles. UEFI only, though
you can modify the script to your own needs.

- [https://wiki.artixlinux.org/Main/Installation](https://wiki.artixlinux.org/Main/Installation)
- [https://wiki.artixlinux.org/Main/InstallationWithFullDiskEncryption](https://wiki.artixlinux.org/Main/InstallationWithFullDiskEncryption)
- [https://wiki.archlinux.org/title/installation_guide](https://wiki.archlinux.org/title/installation_guide)

It implements the dm-crypt encryption technique from this Arch Wiki article: [https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS)

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

You can download this script straight from [red8clover.xyz](https://red8clover.xyz) like so:
```console
curl -LO red8clover.xyz/artix-luks-base-install
```

Then you can proceed with the installation by running the script. The script
takes two parameters, first the disk to wipe & install on and second the
password for the encrypted disk.

```console
chmod +x artix-luks-base-install
sudo su
./artix-luks-base-install /dev/sda myencrypt3dpassw0rd
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
1. Modify sudoers to allow 'wheel' group sudo access with root password only.
1. Set hostname (default 4rt1x).
1. Create /etc/hosts file.
1. Link NetworkManager service to runsvdir.
1. Generate and set default locale to en_US.UTF-8.
1. Set default timezone to Europe and sync hwclock.
1. Perform cleanups.
1. Inform user :)
