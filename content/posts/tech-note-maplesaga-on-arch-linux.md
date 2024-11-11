---
title: "Tech note: MapleSaga on Arch Linux"
date: 2024-11-11T22:19:45+08:00
---

My experience from 2018-2021 running MapleSaga on Arch Linux.

Install:

  - Wine: `wine`, `wine-mono`
  - Multimedia: `lib32-mpg123`
  - Crypto: `lib32-gnutls`

If using PulseAudio, install also `lib32-libpulse` and `lib32-alsa-plugins`.

Optionally install:

  - Winetricks: `winetricks`
  - DirectX: `winetricks d3dx9_43`
  - Font: `winetricks corefonts`

Create wine directory for win32:

```
WINEARCH=win32 WINEPREFIX=~/.maplesaga winecfg
```

Within wine configuration window:

  - Under “Graphics” tab, set DPI to 192.
  - Under “Desktop Integration” tab, disable wine desktop integration and file associations to reduce the number of files created by wine, thus minimising clean-up effort during uninstallation.

Enable font smoothing:

```
cat << EOF > /tmp/fontsmoothing
REGEDIT4

[HKEY_CURRENT_USER\Control Panel\Desktop]
"FontSmoothing"="2"
"FontSmoothingOrientation"=dword:00000001
"FontSmoothingType"=dword:00000002
"FontSmoothingGamma"=dword:00000578
EOF

WINEPREFIX=~/.maplesaga wine regedit /tmp/fontsmoothing 2> /dev/null

rm /tmp/fontsmoothing
```

Install MapleSaga:

```
WINEPREFIX=~/.maplesaga wine saga_setup.exe
```

Get [`ws2_32.dll`](https://www.mediafire.com/?bvt20olayvfbgw7) and [`ws2help.dll`](https://www.mediafire.com/?7c9tee7fhvebopc). Place these files in `$HOME/.maplesaga/drive_c/windows/system32/`.

Set compatibility mode for `MapleSaga.exe` to Windows 98 in `winecfg`.

Create an executable shell script with the following content and place it in `$PATH`:

```
#!/usr/bin/env bash

if [ "$1" = "-k" ]
then
  pgrep -i maple | xargs kill
  exit 0
fi

env WINEDEBUG=fixme-all WINEDLLOVERRIDES=mshtml=d WINEPREFIX="/home/raphx/.maplesaga" wine C:\\\\windows\\\\command\\\\start.exe /Unix /home/raphx/.maplesaga/dosdevices/c:/users/Public/Desktop/MapleSaga.exe.lnk
```

Cleaning up:

```
rm -rf ~/.config/menus
rm -rf ~/.local/share/applications/wine
rm -rf ~/.local/share/desktop-directories
rm -rf ~/.local/share/icons
rm -rf ~/.maplesaga
rm -rf ~/.cache/winetricks
```

Remove:

  - Installed system dependencies
  - The executable shell script
  - Inclusion of `multilib` repository in `pacman` configuration
  - Browser adblocker configuration for `gtop100.com`
