# Pi Based Remote Embedded Debugging

![iPad to Raspberry Pi over USB debugging ARM Cortex M4 with J-Link]()

- Cabled or wireless
- Headless using SSH
- On board ARM Cortex R/M compiling
- On board J-Link support for USB debuggers
- Snazzy terminal based working environment
- Secure user space for storing and building code

Ever wanted to debug an embedded target remotely? Perhaps from an unconventional such as an iPad. Maybe you want to avoid moving your setup from production to your desk, or maybe you're working on a ground isolated system and can't easily plug your laptop into it.

This guide shows you how to set up a Raspberry Pi Model 4B for full embedded development and debugging. Thanks to all major tools now being available as ARM64 binaries, it is easy to set up.

## You will need

- **Raspberry Pi 4B**  (2GB+)
- **Micro SD Card** (16GB+)
- **J-Link debugger** (Or supported devkit such as the [nRF52-DK](https://www.nordicsemi.com/Software-and-Tools/Development-Kits/nRF52-DK))
- **Terminal app** (we like: [Blink](https://blink.sh) *iOS*, [iTerm2](https://iterm2.com) *MacOS*, and [Hyper](https://hyper.is) *Windows/Linux/MacOS*)

## Prepare the SD Card

1. Install the latest [Raspberry Pi OS 64 bit](https://downloads.raspberrypi.org/raspios_arm64/images/) 

   **Info**: Use the [Raspberry Pi Imager App](https://www.raspberrypi.org/software/) to easily flash your SD card

2. Prepare SD for headless operation over SSH and WiFi

   **Option A**: Linux / MacOS users can run the script

   ```bash
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/siliconwitchery/pi-remote-debugging/main/prep-sd-card.sh)"
   ```

   **Option B**: Or do it manually

   1. Create an empty file on the SD card called `ssh`
   2. Create a file on the SD card called `wpa_supplicant.conf` with the contents shown [here](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md)

3. Insert the SD card into your Pi and wait for it to boot

4. Connect to the Pi using your favourite terminal app. Password is `raspberry`

   ```bash
   ssh pi@raspberrypi.local
   ```

#### If it doesn't work

- Re-create the `wpa_supplicant.conf` file with correct WiFi credentials
- DNS spoofing errors can be fixed by removing old entries from the `~/.ssh/known_hosts` file on your local machine

## Setup the Pi

If you're a new Linux user, run all the scripts for a good starting build. Otherwise you can pick and choose which you need. Be sure to read the scripts to details on how the setup works. They are well commented.

**Warning**: The following commands are potentially dangerous. They call a scripts from the internet that are allowed to do anything to your system. You should read the scripts before executing them.

1. Update your system. Takes a while ☕️ will **reboot the pi**

   ```bash
   sudo sh -c "$(curl -fsSL https://raw.githubusercontent.com/siliconwitchery/pi-remote-debugging/main/update-pi.sh)"
   ```

2. Improve security

   ```bash
   sudo sh -c "$(curl -fsSL https://raw.githubusercontent.com/siliconwitchery/pi-remote-debugging/main/pi-security.sh)"
   ```

3. Enable USB tethering

   ```bash
   sudo sh -c "$(curl -fsSL https://raw.githubusercontent.com/siliconwitchery/pi-remote-debugging/main/pi-usb-gadget.sh)"
   ```

4. Install ARM tools

   ```bash
   sudo sh -c "$(curl -fsSL https://raw.githubusercontent.com/siliconwitchery/pi-remote-debugging/main/install-arm-tools.sh)"
   ```

5. Install J-Link tools

   ```bash
   sudo sh -c "$(curl -fsSL https://raw.githubusercontent.com/siliconwitchery/pi-remote-debugging/main/install-arm-tools.sh)"
   ```

6. Add a nicer shell

   ```bash
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/siliconwitchery/pi-remote-debugging/main/zsh-setup.sh)"
   ```

7. Install a nicer editor *Takes a while* ☕️

   ```bash
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/siliconwitchery/pi-remote-debugging/main/install-nvim.sh)"
   ```

## Housekeeping

1. Update the default `raspberry` password using the command

   ```
   passwd
   ```

2. Change the host name from `raspberrypi.local` to something else using

   ```
   sudo raspi-config
   # [Network Options] -> [Hostname] -> change it -> reboot
   ```

   

## Good to go

#### Connecting over USB

The `usb-tether.sh` script sets up a static IP that allows you to connect to the Pi over its USB-C port. The Pi shows up as an ethernet device and can be connected to using the address `10.55.0.1`

```bash
ssh pi@10.55.0.1
```

#### Some handy commands

```bash
# Calling the ARM GCC compiler
arm-none-eabi-gcc

# Running J-Link commander
JLinkExE 

# For working in multiple tabs/windows inside the terminal
tmux

# For editing files
nvim

# Some handy aliases for various common tasks (change or add more in ~/.zshrc)
# git status
# git fetch & pull
# git switch branch
# git add all and commit
# shutdown
# reboot
```

#### Other handy links

- tmux cheatsheet
- Vim cheatsheet

## Shoutouts

This guide was peiced together from various articles. Check them out here
- [Blog post: **Using a Raspberry Pi as a remote headless J-Link Server** by Niall Cooling](https://blog.feabhas.com/2019/07/using-a-raspberry-pi-as-a-remote-headless-j-link-server/)
- [Youtube: **My Favourite iPad Pro Accessory: The Raspberry Pi 4** by Tech Craft](https://www.youtube.com/watch?v=IR6sDcKo3V8&t=3s)
- [Blog post: **Pi4 USB-C Gadget** by Ben Hardill](https://www.hardill.me.uk/wordpress/2019/11/02/pi4-usb-c-gadget/)

## Licence

These instructions and scripts are released unencumbered into the public domain.

Feel free to use this information as a starting point, suggest improvements, or fork this repository so that others may find your version.

All other software installed using these scripts remain under the terms of their respective licenses. 

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
