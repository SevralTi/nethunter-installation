# NetHunter installation

## Intro

We'll install it with **[Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)**

This instruction is for OnePlus6(T), but can be reproduced on the other devices (btw you need special nethunter kernel)

## Stage 0: Installing dependences

Install [DJY's](https://github.com/johanlike/DJY-Oneplus6-or-Oneplus6T-Nethunter-Andrax-Kernel) Android 9 kernel ([click to download](https://drive.google.com/file/d/1FrT25hp5VJnPVzrlnRIOBt3qpWHR811i) - Magisk v20.1 included) ***OR*** [kimocoder's](https://github.com/kimocoder) Android 10 kernel ([click to download](https://github.com/kimocoder/kernel_oneplus_sdm845))

Install [Magisk module by DJY](https://drive.google.com/file/d/1SLgsikGeNoz5Rc6iGLm3JsmkOTRtXDU3) or [modified one by me](https://github.com/AlexeyZavar/nh-magisk-module) **from Magisk Manager** or install Full Kali NetHunter

You're ready!

## Stage 1: Downloading rootfs & Linux Deploy

Download & install [Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)

Download [kalifs](https://build.nethunter.com/kalifs/kalifs-latest/kalifs-armhf-full.tar.xz) and place in at */sdcard/kalifs-armhf-full.tar.xz* (*/storage/emulated/0/kalifs-armhf-full.tar.xz*)

Using any root file explorer, create folders at "*/data/local/nhsystem/kali-armhf*"

## Stage 2: Installing Nethunter

Open **Linux Deploy** and set settings as shown on the [pictures (click)](https://imgur.com/a/6DxbfAQ)

Click on 3 dots in top right corner -> Install

Wait 5-7 minutes

Then click STOP, wait, START

## Stage 3: Configuring Nethunter

Open Terminal -> Android SU

Click on 3 dots in top right corner -> Config Chroot Path and replace "*arm64*" with "*armhf*"

Restart Terminal -> Kali

Finally, write "**apt update && apt upgrade -y && apt dist-upgrade -y**" to update Nethunter

Now you can safely delete **Linux Deploy**

## Troubleshooting

#### apt update

If you'll try to "**apt update**" - you may get an error

To fix that write "**apt-key adv --keyserver hkp://keys.gnupg.net --recv-keys 7D8D0BF6**", but if it doesn't works - replace "*7D8D0BF6*" with key that can't be verified (specified in "apt update" error)

#### ssh

If you need to start ssh - "**service ssh start**"

But you may can't connect to your phone

To fix that write "**nano /etc/ssh/sshd_config**" and find "*PermitRootLogin yes*" - uncomment it. Now find "*UsePAM yes*" and replace it with "*UsePAM no*"

Now "**passwd**" to change root password if needed & "**service ssh restart**"

#### iptables

Nethunter contains 2 versions of "**iptables**", so default doesn't works for me

Simply write "**mkdir -p /root/scripts  && mv /usr/sbin/iptables /root/scripts/ && ln -s /usr/sbin/iptables-legacy /usr/sbin/iptables**"

#### VNC Server & Terminal window

P.S. Thanks to @SymbianSyMoh

For the people who were asking regarding the packages that breaks the terminals title window and other windows title bars, just avoid installing **ANYTHING** related to *XFCE window manager*, this need some careful as some related packages can be installed without your consent for example (when installing **kali-linux-nethunter** package), so the suggestion is to have a ***clean*** Linux Deploy installation with *LXDE window manager* and then to be careful with the anything packages related to *XFCE-\* window manager*.

As the other issue of why sometimes the *VNC* is breaking, also avoid anything related to **TigerVNC**, just don’t install it or anything related and if installed, just remove it (**apt-get purge tiger\* -y**)

#### /etc/profile: Required key not available

Just write "**source /etc/profile && source /root/.bash_profile**"

#### HID

Open Nethunter app -> USB Army -> USB Interface -> hid -> Set USB interface

*or*

Open Terminal -> Android SU and write:

```sh
setprop sys.usb.config win,hid
setprop sys.usb.config win,mass_storage
setprop sys.usb.config win,rndis
setprop sys.usb.config win,hid,mass_storage
setprop sys.usb.config win,rndis,hid
setprop sys.usb.config win,rndis,mass_storage
setprop sys.usb.config win,rndis,hid,mass_storage
setprop sys.usb.config mac,hid
setprop sys.usb.config mac,mass_storage
setprop sys.usb.config mac,ecm
setprop sys.usb.config mac,hid,mass_storage
setprop sys.usb.config mac,ecm,hid
setprop sys.usb.config mac,ecm,mass_storage
setprop sys.usb.config mac,ecm,hid,mass_storage
setprop sys.usb.config win,hid,adb
setprop sys.usb.config win,mass_storage
setprop sys.usb.config win,rndis
setprop sys.usb.config win,hid,adb,mass_storage
setprop sys.usb.config win,rndis,hid,adb
setprop sys.usb.config win,rndis,mass_storage
setprop sys.usb.config win,rndis,hid,adb,mass_storage
setprop sys.usb.config mac,hid,adb
setprop sys.usb.config mac,mass_storage
setprop sys.usb.config mac,ecm
setprop sys.usb.config mac,hid,adb,mass_storage
setprop sys.usb.config mac,ecm,hid,adb
setprop sys.usb.config mac,ecm,mass_storage
setprop sys.usb.config mac,ecm,hid,adb,mass_storage
```

HID interface may not be displayed in Nethunter, but it does exists :)

## Decorations

### Zsh

#### Installation

Do "**apt install zsh -y**"

Then we need to install [Oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) - "**git clone <https://github.com/robbyrussell/oh-my-zsh.git> && cd oh-my-zsh && ./oh-my-zsh.sh**"

Install it as default shell

#### dikiaap's dot files

I've installed [this](https://github.com/dikiaap/dotfiles) dot files, so they're working almost perfect.

"**git clone <https://github.com/dikiaap/dotfiles>**", then "**cd dotfiles && ./install.sh && zsh**"

You can add your functions in "~/.functions", for example:

```sh
# Update
apt-update() {
    sudo apt update
    sudo apt upgrade -y
    sudo apt dist-upgrade -y
    apt-clean
}
```

If there'll be any problems:

##### z

Run "**wget <https://raw.githubusercontent.com/rupa/z/master/z.sh> && mkdir ~/.zsh/plugins/z && mv z.sh ~/.zsh/plugins/z/z.sh**"

##### .functions_private && .exports_private

Just create these files ("**touch ~/.functions_private && touch ~/.aliases_private**") or remove their references in ~/.zshrc

##### Numpad buttons (PuTTY)

Run "**touch ~/.numpad**", add in end of "*~/.zshrc*" "**source ~/.numpad**" add in this file these lines:

```sh
# Keypad
# 0 . Enter
bindkey -s "^[Op" "0"
bindkey -s "^[Ol" "."
bindkey -s "^[OM" "^M"
# 1 2 3
bindkey -s "^[Oq" "1"
bindkey -s "^[Or" "2"
bindkey -s "^[Os" "3"
# 4 5 6
bindkey -s "^[Ot" "4"
bindkey -s "^[Ou" "5"
bindkey -s "^[Ov" "6"
# 7 8 9
bindkey -s "^[Ow" "7"
bindkey -s "^[Ox" "8"
bindkey -s "^[Oy" "9"
# + -  * /
bindkey -s "^[Ok" "+"
bindkey -s "^[Om" "-"
bindkey -s "^[Oj" "*"
bindkey -s "^[Oo" "/"
```

[Source](https://superuser.com/questions/742171/zsh-z-shell-numpad-numlock-doesnt-work)
