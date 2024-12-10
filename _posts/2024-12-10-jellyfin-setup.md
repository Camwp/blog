---
title: "How to Set Up a Jellyfin Server on a Raspberry Pi"
date: 2024-12-10 9:00:00 +0000
categories: [raspberry-pi, media-server]
tags: [media-server, raspberry-pi, tech, programming, hobbies, movies, DIY-project]
math: true
mermaid: true
image:
  path: /assets/img/2024-12-10-jellyfin-setup/img/jellyfin.png
  alt: "Local and remote media server for Raspberry Pi. (Alternative to Netflix, Hulu, Amazon)"
comments: false
---

Jellyfin is a fantastic open-source media server that allows you to stream your media to various clients without any paywalls or restrictions. Unlike its alternatives, Plex and Emby, Jellyfin is completely free-to-use, making it an excellent choice for Raspberry Pi enthusiasts.

In this guide, we'll show you how to set up Jellyfin on a Raspberry Pi and access it via its web interface.

---

## What is Jellyfin?

Jellyfin is a media server that streams your media content from your Raspberry Pi to a variety of devices. It offers features like:
- Inbuilt DVR and live TV functionality.
- Support for major platforms such as Fire TV, Roku, Kodi, Android TV, iOS, and more.

Jellyfin began as a fork of the Emby project after it became proprietary. While it doesn’t yet support as many client devices as Plex or Emby, it’s constantly improving.

> **Recommendation:** Use a Raspberry Pi 4 for the best performance. While Jellyfin can run on a Raspberry Pi 3, heavy transcoding tasks may cause performance issues.

---

## Equipment You'll Need

### Recommended
- **Raspberry Pi 4** (or newer) [Amazon](#)
- **Micro SD Card** (8GB+) [Amazon](#)
- **Ethernet Cable** or **Wi-Fi Adapter** [Amazon](#)

### Optional
- **Raspberry Pi Case** [Amazon](#)
- **USB Keyboard and Mouse** [Amazon](#)

---

## Preparing Your Raspberry Pi

Before installing Jellyfin, follow these steps to prepare your Raspberry Pi:

### 1. Update Your Operating System
Run the following commands to ensure your Raspberry Pi is up to date:
```bash
sudo apt update
sudo apt upgrade -y
```

### 2. Install Required Packages
Install packages to support HTTPS repositories:
```bash
sudo apt install apt-transport-https lsb-release
```

### 3. Import the Jellyfin GPG Key
Add the Jellyfin signing key:
```bash
curl https://repo.jellyfin.org/debian/jellyfin_team.gpg.key | gpg --dearmor | sudo tee /usr/share/keyrings/jellyfin-archive-keyring.gpg >/dev/null
```

### 4. Add the Jellyfin Repository
Add the Jellyfin repository to your Raspberry Pi:
```bash
echo "deb [signed-by=/usr/share/keyrings/jellyfin-archive-keyring.gpg arch=$( dpkg --print-architecture )] https://repo.jellyfin.org/debian $( lsb_release -c -s ) main" | sudo tee /etc/apt/sources.list.d/jellyfin.list
```

### 5. Update Package List
Refresh the package list to include the new repository:
```bash
sudo apt update
```

### 6. (Optional) Set a Static IP Address
To make reconnecting to your Jellyfin server easier, consider setting a static IP address for your Raspberry Pi.

---

## Installing Jellyfin

With the repository configured, installing Jellyfin is simple:

### 1. Install Jellyfin
Run the following command:
```bash
sudo apt install jellyfin
```

### 2. Jellyfin Setup
During installation:
- A new user `jellyfin` is created to run the software.
- A service is created to manage Jellyfin (it will start automatically on boot).

---

## Accessing the Jellyfin Web Interface

Once installed, access Jellyfin's web interface:

### 1. Find Your Raspberry Pi's IP Address
Run the following command to get your IP address:
```bash
hostname -I
```

### 2. Open the Web Interface
In your web browser, navigate to:
```
http://[IPADDRESS]:8096
```
Replace `[IPADDRESS]` with the IP address from the previous step.

---

## Mounting an External Drive for Jellyfin

If you want Jellyfin to access media from an external drive, follow these steps to mount the drive and set the appropriate permissions:

### 1. Mount the External Drive
First, ensure your external drive is connected to your Raspberry Pi. Find the drive's mount point using:
```bash
lsblk
```
Then mount the drive (replace `<drive>` with your actual device name):
```bash
sudo mount /dev/<drive> /media/<default_user>
```
> "If your drive is already mounted automatically, you may skip this step as it will error."
{: .prompt-info }
---

### 2. Set Permissions for Jellyfin
Allow the `jellyfin` user to access the mounted drive:
```bash
sudo setfacl -m u:jellyfin:rx /media/<default_user>
```


Replace `<default_user>` with the actual username or path where the drive is mounted.

### 3. Add Media to Jellyfin
In the Jellyfin web interface, go to the **Libraries** section and add the path to your external drive as a media source.

---

## Summary

You now have Jellyfin running on your Raspberry Pi! You can stream your media library to various devices and enjoy features like DVR and live TV, all for free. Explore Jellyfin’s settings to customize your experience further.

For more Raspberry Pi projects, check out our [blog](#).

Happy streaming!
