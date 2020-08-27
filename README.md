# Maya 2020 Installation on Manjaro KDE (ArchLinux)

## Overview
The default installation instructions for Maya 2020 on a fresh install of manjaro kde.
```
[ozgur@ozgur lib]$ uname -a
>>Linux ozgur 5.7.14-1-MANJARO #1 SMP PREEMPT Fri Aug 7 10:12:32 UTC 2020 x86_64 GNU/Linux
```
Although the installation docs note that installations are hardware-specific, the instructions are generally unclear.
This document shows the steps I took to install Maya, as well as my recommendations for making the installation process less difficult.

To install and troubleshoot Maya, I followed four documents:
- [Install Maya on Linux using the installation wizard](https://knowledge.autodesk.com/support/maya/troubleshooting/caas/CloudHelp/cloudhelp/2019/ENU/Installation-Maya/files/GUID-10FE31A8-7092-45BE-9E53-44D0D096E431-htm.html)
- [Install Maya on Linux using the RPM utility](https://knowledge.autodesk.com/support/maya/troubleshooting/caas/CloudHelp/cloudhelp/2019/ENU/Installation-Maya/files/GUID-10FE31A8-7092-45BE-9E53-44D0D096E431-htm.html)
- [Additional Linux notes](https://knowledge.autodesk.com/support/maya/troubleshooting/caas/CloudHelp/cloudhelp/2019/ENU/Installation-Maya/files/GUID-D2B5433C-E0D2-421B-9BD8-24FED217FD7F-htm.html)
- [Installing Maya 2020 on Arch Linux](https://medium.com/@llama_9851/installing-maya2020-on-arch-linux-e257ffadd52c)

## Installation Procedure
1. Download the latest version, for me it is: `Autodesk_Maya_2020_ML_Linux_64bit.tgz`
 it can be found in [here](https://manage.autodesk.com/products/maya).
2. untar it on a folder located at any location, desktop...
3. let's see what we have here, we have Setup executable -> execute it using 
   `./Setup`
4. An error as expected, 
   ```Bash
   ./Setup: error while loading shared libraries: libpng15.so.15: cannot open shared object file: No such file or directory
   ```
5. checking pacman for libpng15, no clues...    
6. found it on aur package but gave an error while building.
7. found a little help here [Install libpng15-so-15](https://ubuntuforums.org/showthread.php?t=2138623)
8. Also you can try pamac to install it.
9. Now we can run ./Setup but it responds with an error saying that my OS is not supported.
10. This meant that we have to install  it ourselves, not using installer.
11. First we need to install required dependencies.
```
sudo pacman -Syy libjpeg lib32-libjpeg libjpeg6 openssl-1.0 openssl audiofile xorg-fonts-misc libxp python2 python2-backports ld-lsb lsb-release cpio xorg-fonts-100dpi xorg-fonts-75dpi xorg-fonts gsfonts adobe-source-code-pro-fonts xorg-xlsfonts xorg-fonts-type1 libtiff
```
12. for any errors you can get those faulty dependencies from another location or from another package manager, ie. pamac, yum, or build yourself using AUR.(you will need libpng15, it's accesable from AUR)
13. For little aur rundown, -> download snapshot ->extract -> `makepkg -i`
14. Now we have all our dependencies, cool let us direct back to untarred maya folder.
15. Inside Packages folder we have our .rpm packages, basic idea is to convert them to .tar.xz
16. of course we need alot more little apps to make this work, first one is ruby.
`sudo pacman -Syy ruby`
17. now we can get FPM (Freaking Package Manager), `gem install --no-document fpm`
18. we can check if ruby has installed correctly using `alias fpm="/home/ozgur/.gem/ruby/2.7.0/gems/fpm-1.11.0/bin/fpm"` be careful about the version number and use your home folder `/home/YOURUSERNAME`
19.Check


## Summary of Findings

### Missing Dependencies not Mentioned
The documents do mention that additional dependencies are required, but none of them mention the following, both of which were needed to run Maya successfully:
- `libprcre16`
- `libpng15`
## Continuation
**** 
#### Problems with prime-run
The next thing I tried was to use it with my gpu, no problems at a first glance but space bar menu thingy does not work (all screen goes black but menu stays) also, navigation cube is missing ...
#### Other problems
Space bar menu thingy also does not work whether we use it with prime-run or not
## ManjaroKDE Operating System / Hardware Details
In order to reproduce this issue, I used a system with the following specs:
- brand new installation of Manjaro with kernel 5.7.14 with no additional programs installed or configured
- GPU: GeForce GTX 1050m
```bash
[ozgur@ozgur bin]$ lspci -vnn | grep VGA
00:02.0 VGA compatible controller [0300]: Intel Corporation HD Graphics 630 [8086:591b] (rev 04) (prog-if 00 [VGA controller])
01:00.0 VGA compatible controller [0300]: NVIDIA Corporation GP107M [GeForce GTX 1050 Mobile] [10de:1c8d] (rev a1) (prog-if 00 [VGA controller])

```

 
