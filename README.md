# Minimal Debian Sway

For this minimal sway installation firstly you would need a minimal Debian install. I would suggest when tasksel lists the desktop environments to deselect everything and continue with the minimal installation.

## Packages for the minimal install

* sway
* sway-backgrounds
* swaybg
* swayidle
* swaylock
* waybar
* alacritty
* neovim
* ranger
* librewolf
* docker
* git
* sudo
* pipewire
* wireplumber
* nerd fonts
* neofetch
* pavucontrol

## Installation Process

### SUDO Installation

The minimal install wont have SUDO installed or the user added to the SUDO group. The nex few steps will entail how to do it.

* First we must switch to the root user, we do this by using the following command:
  ```bash
  su -
  ```
* You will then be prompted for the root user password.
* Now we can update our repositories and packages:
  ```bash
  apt update && apt upgrade -y
  ```
* Now that our repositories and packages have been updated we can install SUDO:
  ```bash
  apt install -y sudo
  ```
* After SUDO has been installed we can add our main user to the SUDO group:
  ```bash
  usermod -aG sudo <username>
  ```
* We will have to exit all logged in sessions to apply our modifications, in our case we will have to exit twice, a reboot will also suffice:
  ```bash
  exit
  ```

### Sway Installation
The swaywm installation can begin now, we use the following command to install sway and some extra packages that will help for some functionality.

```bash
sudo apt install -y sway sway-backgrounds swaybg swayidle swaylock waybar alacritty neovim ranger git pipewire wireplumber neofetch pavucontrol
```

* We can launch sway now by simply typing sway and hitting enter or we can edit our /etc/profile to launch sway on login.
* Seeing that Neovim is already installed we can use the nvim command to edit our /etc/profile :
  ```bash
  sudo nvim /etc/profile
  ```
* We then add the following to the end of the file to start the sway session on login:
  ```bash
  if [ -z "${WAYLAND_DISPLAY}" ] && [ "${XDG_VTNR}" -eq 1 ]; then
  exec sway
  fi
  ```
* Now sway will start automatically on login.
* To enable our audio to work we will need to restart our system, open a terminal and enable the wireplumber service as a normal user:
  ```bash
  systemctl --user --now enable wireplumber.service
  ```

We now have a basic working installation of sway, next we will add a browser and add some customization to our install.

## Browser Installation
