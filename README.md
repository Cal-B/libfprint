## Validity Sensor `138a:0090` libfprint driver
### A linux driver for 2016 ThinkPad's fingerprint readers
### Installation process for Thinkpad X1 20FC, T460s
### 138a:0090 Validity Sensors, Inc. VFS7500 Touch Fingerprint Sensor
[This tutorial was invaluable in getting this guide trimmed down](https://glsk.net/2018/05/t460s-fingerprint-reader-in-linux/)

#### Ubuntu/Kubuntu installation
Check the original branch for the general install process

First we need to remove any fingerprint software, including `libpam-fprintd` or any other `fprintd` instance.
 - `sudo apt-get remove libpam-fprintd`
 - `sudo apt list | grep 'finger\|fprintd'`  should return no `libpam-fprintd` or `fprintd` packages

 Now we start to install the package
 - `sudo add-apt-repository -u ppa:3v1n0/libfprint-vfs0090`
 - `sudo apt install libpam-fprintd`
 - `sudo systemctl enable fprintd`
 - `reboot -f` will reboot your computer to enable the `fprintd` service

Let's start enrolling fingerprints! Make sure to wash your hands before starting, moistening your fingertips can help with the process if you run into trouble.
 - `fprintd-enroll -f "right-index-finger" "$replace_with_your_username"` enrolls just the right index finger
  - `for finger in {left,right}-{thumb,{index,middle,ring,little}-finger}; do fprintd-enroll -f "$finger" "$USER"; done` enrolls all fingers. Remove entries if you lack these hands/fingers 
  - `fprintd-verify` tests whether these enrollments were stored and identify successdully


Finally, we edit SDDM to properly utilize the reader
 - `sudo nano /etc/pam.d/sddm`
 - Add `auth      sufficient pam_fprintd.so max-tries=3` below the file header and save
 - `sudo pam-auth-update`, press `ENTER`, and select `Fingerprint Recognition` with the arrow keys and `SPACE`. `ENTER` will close the session. 
 - `reboot -f` for good measure  


