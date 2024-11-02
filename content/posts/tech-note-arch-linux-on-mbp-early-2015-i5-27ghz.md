---
title: "Tech note: Arch Linux on MBP Early 2015 i5 2.7GHz"
date: 2024-11-02T15:10:11+08:00
---

My notes from 2017-2019 running Arch Linux on MacBook Pro Early 2015.

## Booting

For `systemd-boot`, ensure the file `/boot/loader/entries/archlinux.conf` has the content:

```
title Arch Linux
linux /vmlinuz-linux
initrd /intel-ucode.img
initrd /initramfs-linux.img
options root=/dev/sdXx rw
```

In `/boot/loader/loader.conf`:

```
default archlinux
```

Add a hook to update `systemd-boot` boot manager whenever `systemd` is upgraded. In the file `/etc/pacman.d/hooks/100-systemd-boot.hook`, add the following:

```
[Trigger]
Type = Package
Operation = Upgrade
Target = systemd

[Action]
Description = Updating systemd-boot...
When = PostTransaction
Exec = /usr/bin/bootctl update
```

If using `grub`, remove timeout in `/etc/default/grub` to achieve faster boot time:

```
GRUB_TIMEOUT=0
```

## Devices

### Bluetooth

Install `bluez` and `bluez-utils`.

Start Bluetooth service:

```
systemctl start bluetooth.service
```

Use the utility tool `bluetoothctl` to pair with Bluetooth devices.

After paired, `obexftp` can be used to exchange files with the devices.

If Bluetooth is not used, it can be disabled for more power saving. Create a file in `/etc/modprobe.d/bluetooth.conf` with the content:

```
blacklist btusb
blacklist bluetooth
```

### Fan

Install `mbpfan-git`.

Edit the configuration `/etc/mbpfan.conf` with:

```
low_temp = 55
high_temp = 60
```

Enable the service:

```
systemctl enable mbpfan.service
```

### Sound

First, change the default sound card in `/etc/modprobe.d/alsa-base.conf`:

```
options snd_hda_intel index=1,0
```

Install utility tool `alsa-utils` to configure `alsa`.

The above two changes require system reboot.

Control the master volume, the value 75% here is an example of volume output:

```
amixer sset Master 75%
```

To store the configurations, run once:

```
alsactl store
``````

Alternatively, `pulseaudio` can be used as the userspace mixer, along with `pulseaudio-alsa`.

### Facetime HD webcam

Install `facetimehd-firmware` and `bcwc-pcie-git` from AUR.

Unload `bdc_pci`:

```
modprobe -r bdc_pci
```

### Keyboard

To use function keys without pressing the Fn key:

```
echo "options hid_apple fnmode=2" | sudo tee /etc/modprobe.d/hid-apple.conf
```

Modify the `FILES` variable in `/etc/mkinitcpio.conf` to include the modprobe configuration file:

```
FILES=(/etc/modprobe.d/hid-apple.conf)
```

### Graphics

Install `mesa`.

For hardware video encoding and decoding, install `intel-media-driver`, and configure to use iHD driver for VAAPI:

```
export LIBVA_DRIVER_NAME=iHD
```

Verify the installation with `libva-utils`:

```
vainfo
```

For brightness adjustment, install `light`, then add user to the `video` group:

```
usermod -aG video <user>
```

### Disk

Mask `lvm2-monitor.service` if LVM is not used.

### Printer

Install `cups` and start `org.cups.cupsd.service`.

Install the necessary drivers for the printer, e.g. `hplip` for HP printers.

Add a queue for the printer:

```
lpadmin -p <queue> -E -v <uri> -m <model>
```

- `<queue>` can be anything, serves as a label for the printer.
- `<uri>` can be retrieved via `sudo lpinfo -v`.
- `<model>` can be retrieved via `lpinfo -m`. This should match the printer’s brand and product series.

Print a file with:

```
lpr -P <queue> <file>
```

### Android device

Install `android-tools` and `android-udev`, add current user to the group `plugdev`, then re-login.

## Administration

### Firewall

Create chains for tcp and udp:

```
iptables -N TCP
iptables -N UDP
```

Configure default chain:

```
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT
iptables -P INPUT DROP
```

Add rules:

```
iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
iptables -A INPUT -p icmp --icmp-type 8 -m conntrack --ctstate NEW -j ACCEPT
iptables -A INPUT -p udp -m conntrack --ctstate NEW -j UDP
iptables -A INPUT -p tcp --syn -m conntrack --ctstate NEW -j TCP
iptables -A INPUT -p udp -j REJECT --reject-with icmp-port-unreachable
iptables -A INPUT -p tcp -j REJECT --reject-with tcp-reset
iptables -A INPUT -j REJECT --reject icmp-port-unreachable
```

Save the configuration:

```
iptables-save -f /etc/iptables/iptables.rules
```

Enable the service:

```
systemctl enable iptables.service
```

Do the same for IPv6:

```
ip6tables -N TCP
ip6tables -N UDP
ip6tables -P FORWARD DROP
ip6tables -P OUTPUT ACCEPT
ip6tables -P INPUT DROP
ip6tables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
ip6tables -A INPUT -i lo -j ACCEPT
ip6tables -A INPUT -m conntrack --ctstate INVALID -j DROP
ip6tables -A INPUT -s fe80::/10 -p ipv6-icmp -j ACCEPT
ip6tables -A INPUT -p ipv6-icmp --icmpv6-type 128 -m conntrack --ctstate NEW -j ACCEPT
ip6tables -A INPUT -p udp -m conntrack --ctstate NEW -j UDP
ip6tables -A INPUT -p tcp --syn -m conntrack --ctstate NEW -j TCP
ip6tables -A INPUT -p udp -j REJECT --reject-with icmp6-adm-prohibited
ip6tables -A INPUT -p tcp -j REJECT --reject-with tcp-reset
ip6tables -A INPUT -j REJECT --reject-with icmp6-adm-prohibited
```

Save configuration:

```
ip6tables-save -f /etc/iptables/ip6tables.rules
```

Enable the service:

```
systemctl enable ip6tables.service
```

### DNS

DNS can be configured manually, or using `systemd-resolved`.

#### Manual

Add public DNS resolvers to `/etc/resolv.conf`:

```
# Cloudflare
nameserver 1.1.1.1
# Quad9
nameserver 9.9.9.10
# Google
nameserver 8.8.8.8
```

Also prevent programs from dynamically updating the file:

```
chattr +i /etc/resolv.conf
```

Changes from `chattr` can be verified using the `lsattr` command.

#### systemd-resolved

Using `systemd-resolved`, first enable the service:

```
systemctl enable systemd-resolved.service
```

Symlink its `resolv.conf`:

```
ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

Then install `systemd-resolvconf` to replace `openresolv`.

The file `/etc/hosts` can be trimmed by removing static configurations for localhost, since `systemd-resolved` handles localhost lookup natively.

Note that this setup uses the name servers from the gateway for domain name queries, hence name server configurations should be setup up appropriately at the gateway.

By default, `systemd-resolved` uses DNSSEC, hence there might be a noticeable increase in DNS lookup time. To disable DNSSEC, create `/etc/systemd/resolved.conf.d/dnssec.conf` with the content:

```
[Resolve]
DNSSEC=no
```

Then restart `systemd-resolved` service.

### Time synchronization

Enable `systemd-timesyncd`:

```
timedatectl set-ntp true
```

### Wireless regulatory

Install `crda`. For Malaysia country, uncomment `WIRELESS_REGDOM=MY` in `/etc/conf.d/wireless-regdom`.

Reboot after the change.

### Network management

#### netctl

For wireless networking, save this profile to `/etc/netctl/a_wpa_profile`:

```
Description='A wpa psk profile'
Interface=wlp3s0
Connection=wireless
Security=wpa-configsection
IP=dhcp
WPAConfigSection=(
    'ssid="ssid goes here"'
    'key_mgmt=WPA-PSK'
    'psk=psk goes here'
)
```

The PSK for `WPAConfigSection` can be retrieved from the command:

```
wpa_passphrase <ssid> <passphrase>
```

Once the profile is ready, establish the connection:

```
netctl start a_wpa_profile
```

For wired networking, in `/etc/netctl/eth`:

```
Description='Wired network'
Interface=enp1s0
Connection=ethernet
IP=dhcp
```

#### systemd-networkd

For wireless, prepare the network file in `/etc/systemd/network/25-wireless.network`:

```
[Match]
Name=wlp3s0
Type=wlan

[Network]
DHCP=ipv4
```

Enable the service:

```
systemctl enable systemd-networkd.service
```

Configure `wpa_supplicant` in a configuration file, `/etc/wpa_supplicant/wpa_supplicant-wlp3s0.conf`:

```
ctrl_interface=DIR=/run/wpa_supplicant GROUP=network

update_config=1

ap_scan=1

network={
  ssid="..."
  psk=...
}
```

Add current user to the `network` group, so that the user can invoke `wpa_cli` without root privilege:

```
usermod -aG network <user>
```

Enable the wpa_supplicant service:

```
systemctl enable wpa_supplicant@wlp3s0.service
```

### Account management

Add the current user to sudo. Edit sudoer file using `visudo`, with the following content:

```
<username> ALL=(ALL) ALL
```

`<username>` is to be substituted, with the user to be added to sudo.

Configure `sudo` to always prompt for password. Run:

```
visudo -f /etc/sudoers.d/10-timestamp-timeout
```

Then add:

```
Defaults:<username> timestamp_timeout=0
```

`<username>` is also to be substituted here.

With current user added to sudo, consider disabling root account login:

```
passwd -l root
```

### Log management

`journald` can be configured to use only volatile storage to save some disk space. Create a file in `/etc/systemd/journald.conf.d/storage.conf` with the content:

```
[Journal]
Storage=volatile
```

Vacuum to clear old logs:

```
journalctl --vacuum-time=1d
```

### Package management

Use HTTPS mirror to download packages from Arch repositories. The list can be found at https://www.archlinux.org/mirrorlist/all/https/. Install the package `pacman-contrib`, and use `rankmirrors` command to select the best mirror by download speed.

### Power management

Install `tlp`.

Enable `tlp.service` and `tlp-sleep.service`.

Mask `system-rfkill.service` and `system-rfkill.socket`.

## Software

### GTK

Optionally disable recent file history for more privacy:

```
cd $HOME/.local/share
echo > recently-used.xbel
chattr +i recently-used.xbel
```

### Font

Install `noto-fonts` and `noto-fonts-cjk`.

### Input method framework

Install `fcitx`, `fcitx-configtool`, and `fcitx-gtk3`. Configure input method by executing:

```
fcitx-configtool
```

Add a new input method. For Chinese, choose Pinyin. To switch between traditional and simplified Chinese, use the hotkey Ctrl-Shift-f while in Pinyin input mode.

### OpenSSH

In `/etc/ssh/sshd_config`:

- Change the default port of SSH server
- Set `PermitRootLogin` to `no`
- Set `PasswordAuthentication` to `no`

## GUI shell

### X

Install X server and initialization software, `xorg-server` and `xorg-init`. Then, use `i3` as a minimal window manager.

Configure display to use Intel DDX driver, add in `/etc/X11/xorg.conf.d/20-intel.conf`:

```
Section "Device"
  Identifier "Intel Graphics"
  Driver "Intel"
  Option "TearFree" "true"
EndSection
```

Alternatively, without configuring X server to use Intel DDX driver, it will fallback to use KMS, which is recommended for lower output latency, since Intel DDX with `TearFree` increases the latency and memory usage. However, using KMS introduces screen tearing. Installing a compositor, like `compton`, resolves this.

For touchpad configuration, add in `/etc/X11/xorg.conf.d/30-touchpad.conf`:

```
Section "InputClass"
  Identifier "bcm5974"
  Driver "libinput"
  MatchIsTouchpad "on"
  Option "Tapping" "on"
  Option "Natural Scrolling" "on"
EndSection
```

For keyboard, add in `/etc/X11/xorg.conf.d/31-keyboard.conf`:

```
Section "InputClass"
  Identifier "Apple Inc. Apple Internal Keyboard / Trackpad"
  MatchIsKeyboard "on"
  Option "XkbOptions" "caps:escape"
EndSection
```

For proper DPI setting, add in `/etc/X11/xorg.conf.d/90-monitor.conf`:

```
Section "Monitor"
  Identifier "<default monitor>"
  DisplaySize 286 179
EndSection
```

Adjust typematic delay and rate by starting Xserver with additional argument, in `$HOME/.xserverrc`:

```
exec /usr/bin/X -nolisten tcp -ardelay 200 -arinterval 33 "$@"
```

### Wayland

Install `sway` and optionally `xorg-server-xwayland` for compatibility with X programs. With XWayland, the `Xresources` file has to be renamed to `Xdefaults` to be used.
