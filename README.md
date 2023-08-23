# Minimal Debian Sway

For this minimal sway installation firstly you would need a minimal Debian install. I would suggest when tasksel lists the desktop environments, deselect everything and continue with the minimal installation.

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
* docker
* git
* sudo
* pipewire
* wireplumber
* nerd fonts
* neofetch
* pavucontrol
* firefox-esr
* fuzzel

## Installation Process

### SUDO Installation

The minimal install won't have SUDO installed or the user added to the SUDO group. The next few steps will entail how to do it.

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
* We will have to exit all logged-in sessions to apply our modifications, in our case we will have to exit twice, a reboot will also suffice:
  ```bash
  exit
  ```

### Sway Installation
The swaywm installation can begin now.

* We use the following command to install sway and some extra packages that will help with some functionality:
  ```bash
  sudo apt install -y sway sway-backgrounds swaybg swayidle swaylock waybar alacritty neovim ranger git pipewire wireplumber neofetch pavucontrol fuzzel firefox-esr
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

We now have a basic working installation of sway, next we will add some customization to our installation.

## Customization

### Fonts
For the customization, we will install the full Nerdfonts font pack.
* Clone the repository:
  ```bash
  git clone https://github.com/ryanoasis/nerd-fonts.git
  ```
* To install, cd into the cloned repository and run the following:
  ```bash
  ./install.sh
  ```

### Customize Sway:
* Clone this repository:
  ```bash
  git clone https://github.com/matthee500/sway-dotfiles
  ```
* Copy all the contents of the config-files folder into the ~/.config folder:
  ```bash
  cp config-files/* ~/.config/
  ```

### Install VSCode
When you install the vscode .deb package or setup the repository and install vscode, it will look for Xorg to start so we will have to edit the startup config of vscode to use Wayland.

* First to install vscode:
   ```bash
   sudo apt install wget gpg
   ```
   ```bash
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
   ```
   ```bash
   sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
   ```
   ```bash
   sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
   ```
   ```bash
   rm -f packages.microsoft.gpg
   ```
   ```bash
   sudo apt install apt-transport-https
   ```
   ```bash
   sudo apt update
   ```
   ```bash
   sudo apt install code # or code-insiders
   ```
* Now we need to edit `/usr/share/applications/code.desktop` to enable the Wayland functionality:
  ```
  [Desktop Entry]
  Name=Visual Studio Code
  Comment=Code Editing. Redefined.
  GenericName=Text Editor
  Exec=/usr/share/code/code --unity-launch %F --ozone-platform-hint=auto --enable-features=WaylandWindowDecorations
  Icon=vscode
  Type=Application
  StartupNotify=false
  StartupWMClass=Code
  Categories=TextEditor;Development;IDE;
  MimeType=text/plain;inode/directory;application/x-code-workspace;
  Actions=new-empty-window;
  Keywords=vscode;
  
  [Desktop Action new-empty-window]
  Name=New Empty Window
  Exec=/usr/share/code/code --new-window %F --ozone-platform-hint=auto --enable-features=WaylandWindowDecorations
  Icon=vscode
  ```

### Install Obsidian
When you install the Obsidian .deb package, it will look for Xorg to start so we will have to edit the startup config of vscode to use Wayland.

* Use the following to install the Debian package that was downloaded from the Obsidian website:
  ```bash
  sudo dpkg -i <package.deb>
  ```
* Now we need to edit `/usr/share/applications/obsidian.desktop` to enable Wayland functionality:
  ```
  [Desktop Entry]
  Name=Obsidian
  Exec=/opt/Obsidian/obsidian %U --ozone-platform-hint=auto --enable-features=WaylandWindowDecorations
  Terminal=false
  Type=Application
  Icon=obsidian
  StartupWMClass=obsidian
  Comment=Obsidian
  MimeType=x-scheme-handler/obsidian;
  Categories=Office
  ```
