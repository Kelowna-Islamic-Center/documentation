# Kiosk Display Guide

This guide provides recommendations for setting up and running the Kiosk App on a dedicated masjid display screen.

There is no single correct way to configure the app, the steps outlined here are just to assist IT administrators or masjid maintainers in deploying new prayer displays within prayer halls. Please note that this process requires technical knowledge and should not be attempted by someone without experience in system administration.

---

## Recommended System Setup

To run it on a display, its recommended you use a lightweight system with minimal background processes so that all resources are dedicated to running just the app.

### Hardware Recommendations

- **Mini PC (Intel Compute Stick)** or **Raspberry Pi 4/5** with at least 2GB RAM
- **Stable Wi-Fi or Ethernet connection**
- **A display that is ideally 1080p or higher. OLED is discouraged but the app does have a screensaver mode to preserve OLED screens.**

### Operating System Options

- **Lightweight Linux Distro**  
  Recommended: [Lubuntu](https://lubuntu.me/) or [Raspberry Pi OS Lite + minimal desktop]  
  These provide a stable and lightweight environment with minimal overhead.

- **Windows (Stripped/Minimal Installation)**  
  A lightweight Windows installation can also be used, but it should be configured to disable unnecessary startup apps, automatic updates, and background processes.

---

## Installing Chrome and Configuring Auto Launch

The kiosk relies on running Google Chrome or an equivalent browser in kiosk mode for a full-screen prayer clock.

=== "Linux (Debian)"
    
    1. Install Chrome

        ```bash
        # Ubuntu/Debian based distros
        sudo apt update
        sudo apt install -y wget gnupg
        wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
        sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list'
        sudo apt update
        sudo apt install -y google-chrome-stable
        ```

    2. Create a new systemd service:

        ```bash
        sudo nano /etc/systemd/system/kiosk.service
        ```

    3. Add the following content:

        ```ini
        [Unit]
        Description=KIC Kiosk
        After=network.target

        [Service]
        ExecStart=/usr/bin/google-chrome --kiosk "https://kelownaislamiccenter.web.app/"
        Restart=always
        User=pi
        Environment=DISPLAY=:0

        [Install]
        WantedBy=graphical.target
        ```

    4. Enable and start the service:

        ```bash
        sudo systemctl enable kiosk.service
        sudo systemctl start kiosk.service
        ```

=== "Windows"

    1. Download Chrome from [google.com/chrome](https://www.google.com/chrome/) and install it as usual.
    2. Open **Task Scheduler**.
    3. Create a new task:
        - Trigger: **At Startup**
        - Action: Run `chrome.exe` with arguments:

            ```
            --kiosk "https://kelownaislamiccenter.web.app/"
            ```

    4. Save the task. On reboot, Chrome will launch directly into kiosk mode.

=== "Raspberry Pi"
    1. Install Chrome

        ```bash
        sudo apt update
        sudo apt install -y chromium-browser
        ```

    2. Edit the autostart config

        ```bash
        nano ~/.config/lxsession/LXDE-pi/autostart
        ```

    3. Add the following lines

        ```bash
        @chromium-browser --kiosk "https://kelownaislamiccenter.web.app/" --noerrdialogs --disable-infobars --check-for-update-interval=31536000
        ```

    4. Save and reboot

        ```bash
            sudo reboot
        ```

---

## Best Practices

* Disable any form of sleep/hibernate on the machine to prevent interruptions.
* Use a wired ethernet connection for reliability.
* Configure auto-reboot in BIOS after power loss for unattended recovery.
* Reboot and physically clean the device occationally
