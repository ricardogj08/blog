# Instalación de Arch Linux

4 de Marzo del 2022

`loadkeys es`

`loadkeys la-latin1`

Conexión por wifi:

```shell
# iwctl
> device list
> station DISPOSITIVO scan
> station DISPOSITIVO get-networks
> station DISPOSITIVO connect RED
> station DISPOSITIVO show
> quit
```

Verificar la conexión:

`# ping -c 5 archlinux.org`

Actualizar el reloj del sistema:

```shell
# ln -sf /usr/share/zoneinfo/REGIÓN/CIUDAD /etc/localtime
# timedatectl set-ntp 1
```

Identificar el nombre de la unidad de almacenamiento:

`# lsblk`

Borrar la tabla de particiones:

`# dd if=/dev/zero of=/dev/sda bs=1M count=8`

Verificar el modo de arranque si es `UEFI` o `BIOS`:

`# ls /sys/firmware/efi/efivars`

Crea una tabla de particiones de tipo `dos`:

`# cfdisk /dev/sda`

UEFI:

<table>
<tr>
<th>Partición (primary)</th>
<th>Punto de montaje</th>
<th>Tamaño</th>
<th>Tipo</th>
</tr>
<tr>
<td>/dev/sda1</td>
<td>/boot</td>
<td>1 GB</td>
<td>Linux</td>
</tr>
<tr>
<td>/dev/sda2</td>
<td>/boot/efi</td>
<td>200 MB</td>
<td>EFI (FAT-12/16/32)</td>
</tr>
<tr>
<td>/dev/sda3</td>
<td>/</td>
<td>------</td>
<td>Linux</td>
</tr>
</table>

Formatear las particiones:

```shell
# mkfs.ext4 /dev/sda1
# mkfs.ext4 /dev/sda3
```

Partición `EFI`:

`# mkfs.fat -F 12 /dev/sda2`

Monta la partición `raíz` y `boot`:

```shell
# mount /dev/sda3 /mnt
# mkdir /mnt/boot
# mount /dev/sda1 /mnt/boot
```

Partición `EFI`:

```
# mkdir /mnt/boot/efi
# mount /dev/sda2 /mnt/boot/efi
```

`# lsblk -f /dev/sda`

`# nano  /etc/pacman.d/mirrorlist`

`# free -h`

Define el tamaño del archivo `swap`:

<table>
<tr>
<th>Memoria RAM</th>
<th>Tamaño del archivo swap</th>
</tr>
<tr>
<td>Menos de 2GB</td>
<td>2 veces del tamaño en RAM</td>
</tr>
<tr>
<td>2GB y menor que 8GB</td>
<td>Igual al tamaño en RAM</td>
</tr>
<tr>
<td>8GB y menor que 64GB</td>
<td>La mitad del tamaño en RAM</td>
</tr>
<tr>
<td>Más de 64GB</td>
<td>Depende de las tareas que realices, si es una estación de trabajo o servidor</td>
</tr>
</table>

Consulte el tamaño de los bloques recomendado de tu ordenador en bytes para generar el archivo `swap`:

```shell
# stat -fc %s .
```

Genera un archivo `swap`:

```shell
# dd if=/dev/zero of=/mnt/swapfile bs=TAMAÑO count=BLOQUES
# chmod 600 /mnt/swapfile
```

> `TAMAÑO` y `BLOQUES` pueden tomar los sufijos b (512 bytes), kB (1,000 bytes), k (1,024 bytes), MB (1,000 kB), M (1,000 k), GB (1,000,000 kB) o G (1,000,000 k).
>
> Para bloques de 4096 bytes (4k) y 8GB (4k \* 2M) en RAM:
>
> `# dd if=/dev/zero of=/mnt/swapfile bs=4k count=2M # Genera un archivo swap de 8GB`

Formatea y activa el archivo `swap`:

```shell
# mkswap /mnt/swapfile
# swapon /mnt/swapfile
```

Instalar:

`# pacstrap /mnt base linux-lts linux-firmware`

`# genfstab -U /mnt >> /mnt/etc/fstab`

`# arch-chroot /mnt`

```shell
# ln -sf /usr/share/zoneinfo/REGIÓN/CIUDAD /etc/localtime
# timedatectl set-ntp 1
# hwclock --systohc --utc
# date
```

Instalar un editor de textos:

`# pacman -S nano nano-syntax-highlighting`


Editar `/etc/locale.gen`:

```
#es_HN ISO-8859-1
es_MX.UTF-8
#es_MX ISO-8859-1
```

```
# locale-gen
# export LANG=es_MX.UTF-8
```

`/etc/locale.conf`

```
LANG=es_MX.UTF-8
```

`/etc/vconsole.conf`

```
KEYMAP=es
KEYMAP=la-latin1
```

Nombre del equipo:

```
echo NOMBRE > /etc/hostname
```


Editar `/etc/hosts`:

```text
127.0.0.1   localhost
::1         localhost
127.0.1.1   NOMBRE
```

Cambiar contraseña:

`# passwd root`

`# mkinitcpio -P`

Instala el gestor de arranque:

`# pacman -S grub`

Para `EFI` es necesario el siguiente paquete:

`# pacman -S efibootmgr`

grub para `BIOS`:

`# grub-install --target=i386-pc /dev/sda`

grub para `UEFI`:

```shell
# grub-install --target=x86_64-efi \
> --efi-directory=/boot/efi \
> --bootloader-id=GRUB
```

Genera un archivo de configuración para grub:

`# grub-mkconfig -o /boot/grub/grub.cfg`

Manuales:

`# pacman -S mandoc`

Gestor wifi:

`# pacman -S iwd`

```shell
# exit
# swapoff -a
# umount -R /mnt
# reboot
```

```text
* Login: root
* Password: ******
```

Identificar el nombre de los dispositivos de red:

`# ip link`

Archivo de configuración para cada uno de las dispositivos de red:

/etc/systemd/network/wlan0.network

```text
[Match]
Name=wlan0

[Network]
DHCP=yes
```

/etc/systemd/network/enp1s0.network

```text
[Match]
Name=enp1s0

[Network]
DHCP=yes
```

```
# systemctl enable systemd-resolved.service
# systemctl start systemd-resolved.service
# ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
# systemctl enable systemd-networkd.service
# systemctl start systemd-networkd.service
# systemctl status systemd-networkd.service
```

Para wifi `/etc/iwd/main.conf`:

`# mkdir /etc/iwd`

```text
[General]
EnableNetworkConfiguration=true
UseDefaultInterface=true

[Network]
RoutePriorityOffset=200
NameResolvingService=systemd

[Scan]
DisablePeriodicScan=true
```

```shell
# systemctl enable iwd.service
# systemctl start iwd.service
# systemctl status iwd.service
```

```shell
# iwctl
> device list
> station DISPOSITIVO scan
> station DISPOSITIVO get-networks
> station DISPOSITIVO connect RED
> station DISPOSITIVO show
> quit
```

Verificar la conexión:

`# ping -c 5 archlinux.org`

Crear un nuevo usuario:

```shell
# useradd -m -G games,uucp,wheel,lp,audio,disk,floppy,input,kvm,optical,scanner,storage,video,network,power -s /bin/bash USUARIO
# passwd USUARIO
```

Cerrar sesión:

```shell
# exit
```

Temperatura:

```shell
$ su -c 'pacman -S thermald'
$ su -c 'systemctl enable thermald.service'
$ su -c 'systemctl start thermald.service'
```

Sonido:

```shell
$ su -c 'pacman -S alsa-utils'
$ amixer sset Master unmute
$ amixer sset Master 80
$ speaker-test -c 2
```

Editar `/etc/pacman.conf`

```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

* https://github.com/goldstar611/gitless

Emulador de terminal:

`$ su -c 'pacman -S foot'`

Launcher:

`$ su -c 'pacman -S bemenu'`

Dependencias del compositor:

`$ su -c 'pacman -S make pkgconf wlroots gcc polkit  qt5-wayland qt6-wayland'`

Instalar el compositor:

`$ curl -fLO https://github.com/djpohly/dwl/archive/refs/tags/v0.2.2.tar.gz `

Fuentes:

https://www.nerdfonts.com/

`$ su -c 'pacman -S zip unzip'`

`$ su -c 'mkdir -p /usr/local/share/fonts'`

`$ su -c 'pacman -S ttf-dejavu'`

`$ su -c 'pacman -Sy dosfstools ntfs-3g'`

Navegador:

`$ su -c 'pacman -S firefox firefox-i18n-es-mx'`

Ofimatica:

`$ su -c 'pacman -S libreoffice-still libreoffice-still-es'`

Diccionarios:

`$ su -c 'pacman -S hunspell-es_mx hyphen-es mythes-es aspell-es'`

Procesos:

`$ su -c 'pacman -S htop'`

Impresoras:

```shell
$ su -c 'pacman -S cups cups-pdf usbutils foomatic-db-engine foomatic-db foomatic-db-nonfree system-config-printer'
$ su -c 'systemctl enable cups.service'
$ su -c 'systemctl start cups.service'
```

Scanner:

```shell
$ su -c 'pacman -S sane-airscan ipp-usb simple-scan'
$ su -c 'systemctl enable ipp-usb.service'
$ su -c 'systemctl start ipp-usb.service'
```

Cortafuegos:

```shell
$ su -c 'pacman -S ufw'
$ su -c 'systemctl enable ufw.service'
$ su -c 'systemctl start ufw.service'
$ su -c 'ufw default deny'
$ su -c 'ufw enable'
```


## Referencias

* [Guía de instalación de Arch Linux.](https://wiki.archlinux.org/title/installation_guide)
* [iwd.](https://wiki.archlinux.org/title/Iwd)
* [Esquema de particiones recomendada por Fedora.](https://docs.fedoraproject.org/en-US/fedora/f35/install-guide/install/Installing_Using_Anaconda/#sect-installation-gui-manual-partitioning-recommended)
* https://wiki.archlinux.org/title/EFI_system_partition
* https://wiki.archlinux.org/title/swap
* https://wiki.archlinux.org/title/Network_configuration#Set_the_hostname
* https://wiki.archlinux.org/title/GRUB
* https://wiki.archlinux.org/title/systemd-networkd
* https://wiki.archlinux.org/title/Systemd-resolved
* https://wiki.archlinux.org/title/Iwd
* https://kisslinux.org/wiki/pkg/eiwd
* https://wiki.archlinux.org/title/users_and_groups
* https://wiki.archlinux.org/title/official_repositories#multilib
* https://wiki.archlinux.org/title/wayland
* https://wiki.archlinux.org/title/fonts
* https://wiki.archlinux.org/title/file_systems
* https://wiki.archlinux.org/title/firefox
* https://wiki.archlinux.org/title/libinput
* https://man.archlinux.org/man/libinput.4
* https://wiki.archlinux.org/title/Xorg/Keyboard_configuration
* https://wiki.archlinux.org/title/LibreOffice
* https://wiki.archlinux.org/title/Language_checking
* https://wiki.archlinux.org/title/List_of_applications
* https://wiki.archlinux.org/title/CUPS
* https://wiki.archlinux.org/title/SANE
* https://wiki.archlinux.org/title/Uncomplicated_Firewall
