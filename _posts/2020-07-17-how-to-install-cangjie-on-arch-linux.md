---
layout: post
title: "How to install cangjie on Arch Linux"
date: 2020-07-17 14:58:55 +0800
categories: "Arch Linux" "cangjie" "ibus" "Chinese Input Method"
---
## The cangjie input method
The cangje input method is a common input method to enter Chinese text into computers. The input method is common among people in Hong Kong, however installing it is quite a hassle compared to Windows or macOS, where it is available when the OS is installed with *Chinese (Hong Kong)* or `zh_HK` locale.

## Implementations
On Linux machines, the cangjie input method is implemented in the [ibus-cangjie][1] library. The ibus-cangjie library, which is implemented under the [ibus framework][2], allows users to input Chinese characters. The ibus framework is designed to enter non-latin characters[^1].

## Installation

### CJK fonts
Before being able to view Chinese characters in a machine installed with Arch Linux, it is required to install a CJK font pack, which is required for any Chinese-Japanese-Korean text to be displayed. If it is already installed, you may skip to the [next session](#installation-ibus-cangjie). To install them, use `pacman`:
```bash
$ pacman -S FONT-PACK
```
Replace `FONT-PACK` with one of the font packages:
 - [Adobe Source Han Sans][3]
 - [Adobe Source Han Serif][4]
 - [Google Noto CJK fonts][5]

### ibus-cangjie {#installation-ibus-cangjie}
Unlike the `ibus-pinyin` or the `ibus-chewing` input method, the [ibus-cangjie][6] is only available with the AUR repository. Therefore installing it would require more work. You may install it with a aur package manager like `aur`:
```bash
$ aur sync ibus-cangjie
$ pacman -S ibus-cangjie
```
Or use the basic `PKGBUILD` method:
```bash
$ git clone https://aur.archlinux.org/ibus-cangjie.git
$ cd ibus-cangjie
$ makepkg -si
```
It will automatically resolve the dependencies (namely the `ibus` package). Note that a super user permission may be required when installing packages.

### Autostarting ibus-cangjie
By default the ibus-cangjie input method will only start when the `ibus-daemon` is called. If the ibus-cangjie input method does not come out after installation, please check whether it persists after running `ibus-daemon`.

Most desktop environments in Linux provide an autostart feature to let users run a program when logged in. To do this, a desktop entry can be added to the user's autostart folder.

1. Create a desktop entry with a `.desktop` file extension (like `ibus-daemon.desktop`) in the `$HOME/.config/autostart/` directory.
2. Edit the file so that it contains the following contents:
    ```config
    [Desktop Entry]
    Type=Application
    Name=ibus-daemon
    Comment=Starts the ibus-daemon on login
    Terminal=true
    Exec=/usr/bin/ibus-daemon -drx
    ```
    The content above tells the desktop environment to run the ibus-daemon command. The `-drx` argument tells it to run the program daemonized (in the background) and execute the ibus XIM server.

## Conclusion
That's it! The cangjie input method is now successfully installed on Arch Linux. By default, you can switch to the cangjie input method with `Win+Space` key combination.


[1]: <https://github.com/Cangjians/ibus-cangjie>
[2]: <https://en.wikipedia.org/wiki/Intelligent_Input_Bus>
[3]: <https://www.archlinux.org/packages/community/any/adobe-source-han-sans-otc-fonts/>
[4]: <https://www.archlinux.org/packages/community/any/adobe-source-han-serif-otc-fonts/>
[5]: <https://www.archlinux.org/packages/extra/any/noto-fonts-cjk/>
[6]: <https://aur.archlinux.org/packages/ibus-cangjie/>

[^1]: See <https://wiki.archlinux.org/index.php/IBus>
