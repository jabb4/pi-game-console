# This is a guide to setup the os for the raspberry pi 5 and to configure it for a game streaming console

1. Use rapsberry pi imager to flash PI 5 with RASPBERRY PI OS LITE (64-BIT)
    > NOTE: Set the settings to configure username, password, wifi, ssh
2. Start you raspberry pi
3. SSH in to the pi
4. Update packages with `sudo apt update && sudo apt upgrade -y`
5. Fix permisions to enable more controller features by running: `sudo usermod -a -G input $USER`
6. Install some packages with `sudo apt install curl pulseaudio`
7. Enable pulseaudio: `sudo systemctl --global enable pulseaudio`
8. Enable autologin by running:
   1. Open raspi config with: `sudo raspi-config`
   2. Navigate to "1 System Options" -> "S6 Auto Login"
   3. Press "Yes", "Ok", "Finish" and reboot now
9.  Install moolight (the streaming client we use to connect to Apollo)
   1. Add Moonlight repo by running: `curl -1sLf 'https://dl.cloudsmith.io/public/moonlight-game-streaming/moonlight-qt/setup.deb.sh' | distro=raspbian codename=$(lsb_release -cs) sudo -E bash`
   2. Install Moonlight by running: `sudo apt install moonlight-qt`
   3. To make Moonlight start at boot we can edit ~/.profile with the command: `nano ~/.profile`
   4. Then, at the button of the file, add:
    ````
    # run moonlight only if we are in a non-ssh interactive shell on login
    if [[ $- == *i* ]] && [[ -z $SSH_CLIENT ]]; then
        moonlight
    fi
    ````
10. Reboot PI: `sudo reboot`
11. If you see a popup saying "No functioning hardware accelerated video decoder was detected.... or the stream lags wile using the settings speciffied bellow you need to follow these steps
    1. Downgrade kernal by running `sudo rpi-update 9c8bf82c0b8e42de75d128118d372a9480225821`
    2. Reboot



## Moonlight settings:
1. When in Moonlight hit the start buton on the controller (This should open up settings panel)
2. Use default settings and change these to:
   1. Resultion: 4K
   2. FPS:60 FPS
   3. Video bitrate: 150 Mbps
   4. Quit app on host PC after ending stream: Checked
   5. Video decoder: Force hardware decoding
   6. Video codec: HEVC (H.265)