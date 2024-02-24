## ForgetMeNotArch
This is a personal guide for Arch Linux, created as a reminder of the key steps in system configuration for the GNOME desktop and AMD GPUs.


 **Optimizing Pacman**

*Parallel Downloads*: Open the file `/etc/pacman.conf` in a text editor and find the line `#ParallelDownloads = 5`. Remove the `#` symbol to uncomment this line, and set the desired number of simultaneous downloads.

*Using Mirrors*: Update the mirror list to speed up package downloads. Use `reflector` to automatically update the mirror list based on speed and stability:
```bash
sudo pacman -S reflector
sudo reflector --verbose --latest 5 --sort rate --save /etc/pacman.d/mirrorlist
```

**Clearing Pacman Cache with paccache**

Before using the `paccache` command, you need to ensure that the `pacman-contrib` package is installed, which includes `paccache` among other utilities. To install it, use the following command:

```
sudo pacman -S pacman-contrib
```

After installing `pacman-contrib`, you can use the `paccache -rk1` command to remove all old cached versions of packages, keeping only the latest version of each. When updating the system or installing packages, `pacman` automatically saves the downloaded packages in a cache. This allows for the restoration of a previous package version if necessary, but over time can significantly increase disk space usage. Using `paccache -rk1` helps free up disk space by deleting old package versions that are no longer needed.

**Adjusting CPU Performance**

- `cpufrequtils` or `cpupower`: These utilities allow you to manage CPU frequency scaling policies, which can impact power consumption.

Install cpupower:
```bash
sudo pacman -S cpupower
```
Configure the scaling policy:
```bash
sudo cpupower frequency-set -g powersave
```
A graphical version of this utility, `cpupower-gui`, can be found in AUR.

**Installing AMDGPU Drivers**

*Install packages*:
```bash
sudo pacman -S xf86-video-amdgpu vulkan-radeon lib32-vulkan-radeon mesa lib32-mesa
```

**Installing VA-API and VDPAU Support**

*Install the VA-API and VDPAU libraries*:
```bash
sudo pacman -S libva-mesa-driver libva-utils mesa-vdpau lib32-mesa-vdpau lib32-libva-mesa-driver
```

**Setting Up Hardware Acceleration in Browsers**

*For Firefox*:
   - Enter `about:config` in the address bar.
   - Search for `gfx.webrender.all` and set it to `true`.
   - Search for `media.ffmpeg.vaapi.enabled` and set it to `true`.

**Verifying Installation**

*Check the installed drivers*:
```bash
lspci -k | grep -EA3 'VGA|3D|Display'
```
Ensure the `amdgpu` driver is being used.

**Enabling and Starting the Bluetooth Service**

Enable and start the systemd service `bluetooth.service` to have Bluetooth operational at every system boot:

```bash
sudo systemctl enable bluetooth.service
sudo systemctl start bluetooth.service
```

**Installing Fish Shell**

Fish (Friendly Interactive SHell) is a user-friendly command-line shell known for its enhancements for interactive work, such as command auto-completion and syntax highlighting.

*Installing Fish*

Open a terminal and run the following command to install Fish through the package manager `pacman`:

```bash
sudo pacman -S fish
```

*Configuring Fish as an Interactive Shell*

Open your `.bashrc` file in a text editor.

```bash
nano ~/.bashrc
```
Add the following lines at the end of the file:

```bash
if [[ $(ps --no-header --pid=$PPID --format=comm) != "fish" && -z ${BASH_EXECUTION_STRING} ]]
then
	shopt -q login_shell && LOGIN_OPTION='--login' || LOGIN_OPTION=''
	exec fish $LOGIN_OPTION
fi
```
Now, each time you open a terminal, it will automatically switch to Fish, while leaving the default Bash shell for scripts and applications.

**Setting Up System Fonts**

*Install a set of fonts*:
```bash
sudo pacman -S noto-fonts ttf-ubuntu-font-family ttf-dejavu ttf-liberation
```
*Use GNOME Tweaks for further font customization*.

**Installing AUR Helpers**

*Installing Paru*:
```bash
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

**Setting Up Temperature Monitoring**

*Install lm_sensors*:
```bash
sudo pacman -S lm_sensors
sudo sensors-detect
```

*Check the temperatures*:
```bash
sensors
```

**Applications: Short Version**

*Install all the listed packages*
```bash
sudo pacman -S btop celluloid discord firefox gnome-shell-extensions gnome-tweaks neofetch nvtop telegram-desktop transmission-gtk wezterm && paru -S pamac-aur
```
Don't forget to install `paru`.

**Applications: Long Version**

*Btop* is a resource monitor that provides detailed information about the use of system resources in an interactive interface.

```bash
sudo pacman -S btop
```

*Celluloid* is a simple and efficient graphical GTK+ interface for the MPV media player.

```bash
sudo pacman -S celluloid
```

*Discord* is a voice, video, and text communication application geared towards the gaming community.

```bash
sudo pacman -S discord
```


*Firefox* is a free and open-source web browser developed by the Mozilla Foundation.

```bash
sudo pacman -S firefox
```

To manage GNOME Extensions locally and adjust them from the system, it's recommended to install *GNOME Shell Extensions** and **GNOME Shell Integration*:

```bash
sudo pacman -S gnome-shell-extensions
```

For integration with a browser (e.g., Firefox or Chrome), you will need to install the corresponding browser extension from their web stores.

*GNOME Tweaks* (Tweaks Tool) is a tool for configuring the GNOME desktop, providing access to features not available through standard GNOME settings.

```bash
sudo pacman -S gnome-tweaks
```

*Neofetch* is a command-line utility that displays information about your system, kernel, desktop settings, themes, etc., in the terminal.

```bash
sudo pacman -S neofetch
```

*Nvtop* is an interactive process monitor for Nvidia GPUs, allowing you to monitor GPU and memory usage in real-time.

```bash
sudo pacman -S nvtop
```

*Pamac* is a graphical package manager designed for Arch Linux and Manjaro, providing a convenient interface for managing packages and access to AUR.

```bash
paru -S pamac-aur
```

*Telegram* is an instant messaging app supporting groups, channels, and bots, known for its speed and security.

```bash
sudo pacman -S telegram-desktop
```

*Transmission* is a lightweight BitTorrent client with a graphical interface, CLI, and web interface.

```bash
sudo pacman -S transmission-gtk
```

*WezTerm* is a GPU-accelerated terminal emulator with tab support and split views, offering extensive customization options.

```bash
sudo pacman -S wezterm
```
