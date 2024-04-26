---
layout: post
title: Installing and configuring Ubuntu 20.04
---

**Content**

- [Introduction](#introduction)
- [Installing Ubuntu](#installing-ubuntu)
- [First update and configuration](#first-update-and-configuration)
- [Installing additional software](#installing-additional-software)
- [References](#references)


<div class="message-info">Under construction.

<p>This post gives a brief overview on how to install Ubuntu on your machine and do some basic configurations.</p>
</div>

<br>

### Introduction

[Linux](https://en.wikipedia.org/wiki/Linux){:target="_blank"} is an open source operating system that powers most of the infrastructure of the internet. Historically, it started as a kernel for Unix like systems by Linus Torvalds, but then gained so much popularity that many operating systems were using it, like Debian, on which Ubuntu is based.

Ubuntu is the most popular  distribution on the planet. It is used by millions of people across the world and it powers millions of servers, workstations and laptops.

Ubuntu comes in three versions : Desktop, Server and Core. It has a LTS (Long Term Support) version that comes every two years, and an in-between versions that are released every six months.

If you have a laptop or a tower then Ubuntu Desktop is what you are looking for. If you want to install Ubuntu server, then you likely know how to install Ubuntu and this post is not for you. We recommend installing an LTS (Long Term Support) version, since they are the most stable releases that will receive support for 5 years. Ubuntu 20.04 will receive updates and security patches until April 2025. At which point, we recommend switching to the 22.04 release. Why not directly install Ubuntu 22.04 ? Well, it's a choice ! This way however, the [community](https://askubuntu.com/){:target="_blank"} has the time to catch up and put content on the internet on that version. Also, you can be sure the version of Ubuntu you have have plenty of security patches already developed.


**Downloading the *.iso* file**

Go to [https://ubuntu.com/download/alternative-downloads](https://ubuntu.com/download/alternative-downloads){:target="_blank"}. In the page, a list of options are available. Choose Ubuntu 20.04 torrent file :

![iso_file_download](/pictures/Screenshot_from_2024-04-17_20-13-22.png)

You could download the *iso* file directly in your browser, but using a torrent client application is better.

If you prefer to download Ubuntu image from the command line, you could type in the following command :

	wget -c https://releases.ubuntu.com/20.04.6/ubuntu-20.04.6-desktop-amd64.iso

The -c option is to be able to continue the download if it is interrupted.

If you are on Windows, you can use [wget for windows](https://gnuwin32.sourceforge.net/packages/wget.htm){:target="_blank"}.

**Verifying the iso file**

Once the download is complete, we can verify that it doesn't have any errors and that it is legitimate.

First we need to verify that the checksum is indeed signed by ubuntu. Run the following command :

	 gpg --keyid-format long --verify SHA256SUMS.gpg SHA256SUMS

Copy the *long_hex_number* you get, prefix it with 0x then run : 

	gpg --keyid-format long --keyserver hkp://keyserver.ubuntu.com --recv-keys 0xlong_hex_number

We can then inspect the key fingerprints with :

	gpg --keyid-format long --list-keys --with-fingerprint 0xlong_hex_number

You should get something like this :

	pub   rsa4096/long_hex_number 2012-05-11 [SC]
      Key fingerprint = 8439 38DF 228D 22F7 B374  2BC0 D94A A3F0 EFE2 1092
	uid   Ubuntu CD Image Automatic Signing Key (2012) <cdimage@ubuntu.com>

To verify the checksum file using the signature :

	gpg --keyid-format long --verify SHA256SUMS.gpg SHA256SUMS

Which should produce something like the following :

	gpg: Signature made Fri 25 Mar 04:36:20 2016 GMT
                    using RSA key ID long_hex_number
	gpg: Good signature from "Ubuntu CD Image Automatic Signing Key (2012) <cdimage@ubuntu.com>" [unknown]
	gpg: WARNING: This key is not certified with a trusted signature!
	gpg:          There is no indication that the signature belongs to the owner.
	Primary key fingerprint: 8439 38DF 228D 22F7 B374  2BC0 D94A A3F0 EFE2 1092

If you get **Good** signature, it means that the checksum is indeed signed by ubuntu.

To verify the *.iso* file, type in : 

	 sha256sum -c SHA256SUMS 2>&1 | grep OK

Of course, the *.iso* file needs to be in the same directory as SHA256SUMS and SHA256SUMS.gpg files.

You should get something like the following : 

	ubuntu-20.04.6-desktop-amd64.iso: OK

Getting a different result indicates that your file is corrupted and you need to download it again from a trusted source.

<br>

### Installing Ubuntu

<div class="message-warning">Before Installing Ubuntu make sure you make a copy of your important documents and folders. You may loose your data during the installation process.</div>

Installing Ubuntu is pretty easy, you can do it in a couple of minutes.

After downloading and verifying the *.iso* file, you need to create a bootable usb with it.

Go to [Balena Etcher download](https://etcher.balena.io/#download-etcher) section and download the file corresponding to your OS.

Once Etcher is installed, insert the usb thumb drive (which should be at least 4G), select the *iso* file then select the drive, then click flash and wait for Etcher to finish.

On reboot make sure the boot order in the BIOS settings is set to boot from the usb thumb drive. This is usually done using the F12 key but it may differ from model to model.

In the first screen of the installation process, select install Ubuntu :

![install1](/pictures/ubuntu-20.04_19_04_2024_10_16_14.png)

Then select normal or minimal installation. Minimal installation may take a bit longer since the installer removes many default applications. If you are connected to the internet, we recommend selecting the *Download updates while installing* option.

![install2](/pictures/ubuntu-20.04_19_04_2024_10_29_43.png)

You can either choose to install Ubuntu on the entire disk, alongside another OS like Windows or do a custom installation. In this tutorial, we just install Ubuntu on the entire disk, so select the *Erase disk and install Ubuntu* option and validate :

![install3](/pictures/ubuntu-20.04_19_04_2024_10_31_36.png)

Pick a username, this will be the one you login with in your computer. By default it has Administrator privileges, which means we can do *sudo* stuff with it. Pick a strong password, then validate.

![install4](/pictures/ubuntu-20.04_19_04_2024_10_32_50.png)

Enjoy a cup of Coffee while the installer is copying your files :)

![install5](/pictures/ubuntu-20.04_19_04_2024_10_38_00.png)

Don't forget to remove the usb stick before restarting :

![install6](/pictures/ubuntu-20.04_19_04_2024_11_39_46_1.png)

<br>

### First update and configuration

**Updating the system**

To update Ubuntu, type in :

	sudo apt-get update && sudo apt-get upgrade

**Firewall**

The default firewall for ubuntu is [UFW](https://help.ubuntu.com/community/UFW). We are not going to cover most of its features, maybe in a future post.

For now just deny incoming requests :

	 sudo ufw default deny incoming

Deny ssh connections

	sudo ufw deny ssh

Enable the service at startup

	sudo ufw enable

**Managing Users**

Managing users is a critical component of your istallation. Depending on what you want to do, we recomand following the security guides provided by Ubuntu.
For instance : [User management](https://ubuntu.com/server/docs/user-management){:target="_blank"}.

<br>

**Getting a pro subscription**

First thing to do is to register for a [pro subscription](https://ubuntu.com/pro/subscribe){:target="_blank"}. It is Free for up to five machines.

Once logged-in, copy your token and atach your machine as follow : 

	sudo pro attach your_token
	
Follow the instructions in your terminal, then update and upgrade your system.

If you open "Software and Updates", you should get a new tab "Ubuntu Pro".

![install1](/pictures/ubuntu_pro_tab.png)

And a Liveaptch icon. Livepatch enables security updates that don't require a restart.

![install1](/pictures/livepatch_icon.png)

**Auditing our system against CIS Benchmark**

Enable Ubuntu Security Guide :

	sudo pro enable usg
	sudo apt-get install usg

You can then audit your system for CIS Benchmark :

	sudo usg audit cis_level1_workstation
	
Which will produce the result in an html and xml format located at :

	/var/lib/usg/usg-report-20240426.1128.html

You can inspect the file and install and configure what you want. For example to install [aide](https://askubuntu.com/questions/1507027/how-install-aide-on-ubuntu){:target="_blank"}, type in :

	sudo apt-get install aide
	
	
<div class="message-tip">
<p>Depending on your knowledge and the type of hardening you want to do, you can proceed to do everything yourself, or <a href="https://ubuntu.com/security/certifications/docs/usg/cis/compliance" target="_blank">comply your system to the CIS benchmark automatically</a>. We recommand that you create a copy of your important files and configurations and create an administrator account before doing so. Follow the official guide <a href="https://ubuntu.com/security/certifications/docs" target="_blank">Security Compliance & Certifications for 20.04</a> as you see fit.
</p>
</div>

<br>

**Installing ClamAV**

<div class="message-info">
<p>
<a href="https://www.clamav.net/about">ClamAV</a> is an open-source (GPL) anti-virus engine used in a variety of situations, including email and web scanning, and endpoint security. It provides many utilities for users, including a flexible and scalable multi-threaded daemon, a command-line scanner and an advanced tool for automatic database updates.
</p>
</div>

To install ClamAV type in :
	
	sudo apt-gte install clamav clamav-daemon clamtk

<br>

### Installing additional software

**Installing Dropbox**

Installing Dropbox is somewhat complicated as there is a [bug in the gui process](https://askubuntu.com/questions/154671/dropbox-install-stuck-at-99-how-do-i-fix-it-and-any-dpkg-errors).

First, get the .*deb* file :

	 wget -c https://www.dropbox.com/download?dl=packages/ubuntu/dropbox_2020.03.04_amd64.deb

Then install it using dpkg :

	sudo dpkg -i dropbox_2020.03.04_amd64.deb

When you see the screen saying "Start Dropbox to finish installation", press *Close*.

Install the dropbox daemon :

	 cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -

After restarting your computer, launch Dropbox from the application menu, login to Dropbox on your browser and set things up in the *Preferences* section.

![install7](/pictures/Screenshot_from_2024-04-18_10-16-36.png)

If you don't have a Dropbox account, you can use [this referral link](https://www.dropbox.com/referrals/AAABTrW6_E0coaWvRhRAxegHqD-dgVXkTAM?src=global9) (you will get an additional 500 Mb for free).

**Installing Inkscape**

	sudo apt install inkscape
	
**Installing Gimp**

	 sudo apt install gimp

**Installing VLC Player**

	sudo apt install vlc

**Installing Chromium-browser**

Chromium is the open source version of Google-Chrome browser. To install it, type in :

	sudo apt install chromium-browser

**Installing Obsidian**

Go to the [obsidian website](https://obsidian.md/download) and download the .deb file listed at the end of the site.

Then run the following command : 

	 sudo dpkg -i obsidian_file_name.deb
	 
**Installing VirutalBox**

go to [Virtualbox website](https://www.virtualbox.org/wiki/Linux_Downloads){:target="_blank"} and download the .deb file for Ubuntu 20.04. Also download the sha256 checksum.

Make sure to verify your download.

	sha256sum -c SHA256SUMS 2>&1 | grep OK
	
Which should produce :

	virtualbox-7.0_7.0.16-162802~Ubuntu~focal_amd64.deb: OK
	
Install Virtualbox :

	sudo dpkg -i virtualbox-7.0_7.0.16-162802~Ubuntu~focal_amd64.deb

If for some reason the package has unmet dependencies, you can install them using :

	sudo apt-get install -f
	
Download VirtualBox extensionpack (make sure it is the same version as your VirtualBox) :

	wget -c https://download.virtualbox.org/virtualbox/7.0.16/Oracle_VM_VirtualBox_Extension_Pack-7.0.16.vbox-extpack
	
Open VirtualBox, got to File > Tools > Extension Pack Manager > Install, then locate the file you just downloaded. Follow the instructions to complete the installation.

<br>

### References

- [Installing Ubuntu](https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview){:target="_blank"}
- [How to verify Ubuntu](https://ubuntu.com/tutorials/how-to-verify-ubuntu#1-overview){:target="_blank"}
- [Hardening Ubuntu](https://ubuntu.com/blog/what-is-system-hardening-definition-and-best-practices){:target="_blank"}
