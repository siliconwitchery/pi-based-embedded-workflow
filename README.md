# Pi Based Remote Embedded Debugging

![iPad to Raspberry Pi over USB debugging ARM Cortex M4 with J-Link](raspberry-pi-jlink-debugging.jpg)

This guide is a series of shell scripts which can be run on a **Raspberry Pi 4** to set up a rich workflow for developing and debugging embedded hardware.

**Includes:**

- **Headless operation** over SSH
- **WiFi or wired** connection over USB-C
- **ARM GCC** embedded toolchain
- **J-Link** debugging tools
- **Colourful IDE** style dev environment using [neovim](https://github.com/neovim/neovim) and [tmux](https://github.com/tmux/tmux/wiki)
- **Encrypted user space** for storing SSH keys, source code and sensitive files

**Perfect for:**

- Debugging remote targets
- Debugging ground isolated targets
- Cable free workflows
- Dedicated environments for specific projects
- Ability to easily switch workstation and re-connect from any terminal

## You will need

- Raspberry Pi Model 4B
- MicroSD Card
- J-Link debugger – Or supported devkit such as the [nRF52-DK](https://www.nordicsemi.com/Software-and-Tools/Development-Kits/nRF52-DK)
- Terminal app – Some we like: [Blink](https://blink.sh) (iOS), [iTerm2](https://iterm2.com) (MacOS), and [Hyper](https://hyper.is) (Windows/Linux/MacOS)

## Installation

#### 1. Install Raspberry Pi OS

Get the latest 64 bit build from [here](https://downloads.raspberrypi.org/raspios_arm64/images/), and use the official [Raspberry Pi Imager](https://www.raspberrypi.org/software/) to flash your SD card.

#### 2. Enable SSH and WiFi

On **Linux or MacOS** simply run this from your terminal and follow the steps:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/siliconwitchery/pi-remote-debugging/main/prep-sd-card.sh)"
```

**Or do it manually** by creating an empty file on the SD card called `ssh`, and then create a file called `wpa_supplicant.conf` with the contents shown [here](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md).

#### 3. Boot the Pi and connect

Insert the MicroSD into your Pi and connect over SSH using your favourite terminal app. Password is `raspberry`

```
ssh pi@raspberrypi.local
```

**If it doesn't work**: Your `wpa_supplicant.conf` file might be incorrect. Recreate it. If you get a DNS spoofing error, you'll need to clear out old entries from the `~/.ssh/known_hosts` file on your local machine.

#### 4. Run the setup script

This script serves as a starting point. Try it out, then customise it to your needs.

**Warning**: This has only been tested on a fresh Raspberry Pi OS install. The script often calls **sudo** so you should really read it before running it on a live system.

```bash
sudo sh -c "$(curl -fsSL https://raw.githubusercontent.com/siliconwitchery/pi-remote-debugging/main/setup.sh)"
```



## Housekeeping

#### 1. Update the default `raspberry` password using this command

```
passwd
```

#### 2. Change the host name from `raspberrypi.local` to something else using

```
sudo raspi-config
# [System Options] -> [Hostname] -> change it -> reboot
```

## That's it!

#### Connecting over USB

The `usb-tether.sh` script sets up a static IP that allows you to connect to the Pi over its USB-C port. The Pi shows up as an ethernet device and can be connected to using the address `10.55.0.1`

```bash
ssh pi@10.55.0.1

# Note the connection over WiFi will still work too
```

#### Calling the ARM GCC compiler

It's located inside `/tools/gcc-arm-none-eabi/bin`. The path should already be added inside `.zshrc` so you can simply call:

```bash
arm-none-eabi-gcc
```

#### Calling J-Link tools

It's located inside `/tools/jlink`. The path should already be added inside `.zshrc` so you can simply call:

```bash
JLinkExe
```

#### Working in multiple tabs

Tmux is handy for saving layouts and sessions. Access it with

```bash
tmux

# To create a window to the right

# To create a window below

# To switch windows

# To Resize windows
```

There are many more commands. Check out this [cheat sheet]().

#### Editing files

Neovim is a powerful editor. It's not straightforward to learn, however here are a few links to get you started. Start neovim with

```bash
nvim
```

#### Some extras

These are "aliases" or shortcuts added into the `.zshrc` file for various commands. Add your own as you go

```bash
# Shutdown. Same as 'sudo shutdown now'
sd
```

## Shoutouts

These articles were a great resource when we built this guide. Be sure to go check them out:
- [Blog post: **Using a Raspberry Pi as a remote headless J-Link Server** by Niall Cooling](https://blog.feabhas.com/2019/07/using-a-raspberry-pi-as-a-remote-headless-j-link-server/)
- [Youtube: **My Favourite iPad Pro Accessory: The Raspberry Pi 4** by Tech Craft](https://www.youtube.com/watch?v=IR6sDcKo3V8&t=3s)
- [Blog post: **Pi4 USB-C Gadget** by Ben Hardill](https://www.hardill.me.uk/wordpress/2019/11/02/pi4-usb-c-gadget/)
- [Blog post: **Encrypting your home directory using LUKS on Debian/Ubuntu** by François Marier](https://feeding.cloud.geek.nz/posts/encrypting-your-home-directory-using/)

## Licence

These instructions and scripts are released unencumbered into the public domain.

Feel free to use this information as a starting point, suggest improvements, or fork this repository so that others may find your version.

All other software installed using these scripts remain under the terms of their respective licenses. 

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
