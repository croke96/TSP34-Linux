# TSP34-Linux
TSP34 Linux Debian Edition V1.0

Overview, package list, quick commands and hotkeys.

# **README**

## **What it is**

A custom Debian 13 (trixie) distro with the GNOME desktop, built with live-build. It boots as a live session, so you can try it without installing, and it includes the Debian installer. It comes preloaded with networking, IT troubleshooting, admin and scripting tools, so a fresh install is ready for lab work without hunting down packages. Think of it as Kali-style network and troubleshooting tools without the pentesting suite.

## **What this guide does not cover**

This guide does not include install steps, VM setup or Enhanced Session setup. Installing and configuring a Linux system is part of the assignment, so that part is yours to work out. Once it is installed, this guide covers what is on it and how to start using it.

## **Requirements**

| Item | Requirement |
| :---- | :---- |
| Architecture | 64-bit (amd64), UEFI |
| Memory | 4 GB or more |
| Disk | 40 GB or more |
| Secure Boot | Not supported in v1.0 (must be off) |
| Image | TSP34-Linux-Debian-v1.0.iso (about 3.3 GB, hybrid: boots as DVD or USB) |

## **Defaults**

| Item | Value |
| :---- | :---- |
| Base | Debian 13 (trixie), amd64, GNOME |
| Live session login | tspstudent / live (you must change the password at first login) |
| Installed username | tspstudent (you set the password during install) |
| Root account | Disabled; tspstudent has sudo |
| Language / keyboard / time zone | en\_CA / US / America/Vancouver |
| Hostname | You choose it during install |

## **Preconfigured**

**Look:** TSP34 wallpaper (made for 1920x1200), dark mode, 12-hour clock, battery percentage, minimize and maximize buttons, tap-to-click.

**Dock (left to right):** Brave, Files, Software, Calculator, Wireshark, Terminal, Settings.

**Wireshark:** tspstudent is in the wireshark group, so you can capture packets without sudo.

**Docker:** your user is not in the docker group (membership is effectively root access). Use sudo, or add yourself once you understand the risk.

**Tailscale:** installed but not logged in. Run the command below and finish the login in the browser.

| Bash |
| :---- |
| sudo tailscale up |

**tmux:** mouse support and a 10,000-line history. **vim:** syntax highlighting, line numbers, 4-space tabs.

**SSH:** in the live session, password logins over SSH are off, so nobody can log in remotely with the public live password. Installed systems use normal Debian SSH defaults.

## **Shell aliases**

Defined in \~/.bash\_aliases.

| Alias | Runs | Use |
| :---- | :---- | :---- |
| ll | ls \-alFh | Detailed file list |
| ports | ss \-tulpn | Listening ports and the programs using them |
| myip | ip \-br a | Short, ipconfig-style IP list |
| update | sudo apt update && sudo apt full-upgrade | Update everything |
| cls | clear | Clear the terminal |

## **Good to know**

Live session changes are not saved. Every live boot starts fresh, including the forced password change. SSH host keys also change every live boot, so an SSH client may warn that the host key changed. Clear the old key on the client with:

| PowerShell |
| :---- |
| ssh-keygen \-R VM-IP-ADDRESS |

GNOME allows one desktop session per user. If you are logged in at the console, a remote desktop (RDP) session for the same user shows a black screen. Log out of the console first; locking is not enough.

## **Known issues (v1.0)**

1\. Live session only: RDP login fails with "User is not authorized" until the live password has been changed. Log in at the normal console first, set a new password, log out, then connect again. Installed systems are not affected.

2\. Hostname is not pre-filled in the installer.

3\. Not yet tested on bare metal.

# **Hotkeys**

## **GNOME desktop**

| Keys | Action |
| :---- | :---- |
| Super | Activities overview and search (type an app name, press Enter) |
| Super \+ A | Show all apps |
| Super \+ Left / Right | Snap window to the left or right half |
| Super \+ Up / Down | Maximize / restore window |
| Super \+ H | Hide (minimize) window |
| Super \+ L | Lock screen |
| Super \+ V | Notifications and calendar |
| Alt \+ Tab | Switch apps |
| Alt \+ \` (backtick) | Switch windows of the same app |
| Alt \+ F2 | Run a command |
| Print Screen | Screenshot tool (area, window or screen) |
| Ctrl \+ Alt \+ Shift \+ R | Start / stop a screen recording |

## **Terminal and Bash**

| Keys | Action |
| :---- | :---- |
| Ctrl \+ Shift \+ C / V | Copy / paste in the terminal |
| Ctrl \+ Shift \+ T | New terminal tab |
| Tab (twice) | Complete a command, option or path; show choices |
| Ctrl \+ R | Search command history (fuzzy, through fzf) |
| Ctrl \+ C | Stop the running command |
| Ctrl \+ L | Clear the screen |
| Ctrl \+ A / Ctrl \+ E | Jump to start / end of the line |
| Ctrl \+ U | Delete everything before the cursor |
| \!\! | Repeat the last command (sudo \!\! reruns it with sudo) |

## **tmux**

Every tmux shortcut starts with the prefix Ctrl \+ B, released, then the key.

| Keys | Action |
| :---- | :---- |
| Ctrl \+ B, % | Split left / right |
| Ctrl \+ B, " | Split top / bottom |
| Ctrl \+ B, arrow key | Move between panes (or click, mouse is on) |
| Ctrl \+ B, c | New window |
| Ctrl \+ B, n / p | Next / previous window |
| Ctrl \+ B, z | Zoom the current pane in and out |
| Ctrl \+ B, d | Detach (the session keeps running) |

Reattach to a detached session:

| Bash |
| :---- |
| tmux attach |

# **Package Guide**

Every included package, what it does and one command to try. Replace HOST, eth0 and similar placeholders with your own values; check your interface name with myip. For any tool, man TOOL or TOOL \--help shows the full options.

## **Network tools**

| Package | What it does | Try it |
| :---- | :---- | :---- |
| nmap | Finds hosts, open ports, services and often the OS. | nmap \-sn 192.168.1.0/24 |
| wireshark | Graphical packet analyser that decodes every protocol layer. | Open from the dock |
| tshark | Command-line Wireshark, for captures in a terminal or over SSH. | tshark \-i eth0 \-c 20 |
| tcpdump | Lightweight command-line packet capture. | sudo tcpdump \-i eth0 \-n icmp |
| mtr-tiny | Ping plus traceroute: live latency and loss at every hop. | mtr 1.1.1.1 |
| iperf3 | Measures real bandwidth between two machines. | iperf3 \-s (on one) then iperf3 \-c HOST (on the other) |
| traceroute | Shows each router hop to a destination. | traceroute 8.8.8.8 |
| dnsutils | dig and nslookup, for name resolution problems. | dig bcit.ca |
| whois | Who owns a domain or IP block. | whois bcit.ca |
| ethtool | NIC speed, duplex, driver and error counters. | ethtool eth0 |
| arp-scan | Lists every device on the local network using ARP. | sudo arp-scan \-l |
| fping | Pings many hosts at once, good for a whole subnet. | fping \-a \-g 192.168.1.0/24 |
| iftop | Live bandwidth per connection. | sudo iftop \-i eth0 |
| nethogs | Live bandwidth per program. | sudo nethogs |
| iptraf-ng | Console traffic monitor by protocol, port and interface. | sudo iptraf-ng |
| netcat-openbsd | Raw TCP/UDP connections, to test ports or move data. | nc \-zv HOST 22 |
| socat | Advanced netcat: relays between sockets, files and serial ports. | socat \- TCP:HOST:80 |
| curl | Transfers data to or from URLs; tests web servers and APIs. | curl \-I https\://bcit.ca |
| wget | Downloads files from the web, with resume. | wget \-c URL |
| ipcalc | Subnet calculator: network, broadcast, host range, mask. | ipcalc 192.168.1.0/26 |
| lldpd | Shows which switch and port you are plugged into (if LLDP is on). | lldpcli show neighbors |
| picocom | Serial terminal for console cables to switches and routers. | sudo picocom \-b 9600 /dev/ttyUSB0 |
| tftpd-hpa | TFTP server for moving firmware and configs to network devices. | systemctl status tftpd-hpa |
| wireguard | Tools for the WireGuard VPN. | sudo wg show |
| net-tools | Classic ifconfig, netstat, route and arp. | netstat \-rn |

## **Remote access and file shares**

| Package | What it does | Try it |
| :---- | :---- | :---- |
| openssh-client | ssh, scp and sftp for secure remote connections. | ssh USER@HOST |
| openssh-server | Lets other machines SSH into this one. | systemctl status ssh |
| remmina | Graphical remote desktop client (RDP, VNC, SSH). | Open from Activities |
| remmina-plugin-rdp | Adds RDP support to Remmina. | Pick RDP in Remmina's protocol list |
| freerdp3-x11 | Command-line RDP client. | xfreerdp3 /v:HOST /u:USER |
| smbclient | Command-line access to Windows/Samba shares. | smbclient \-L //HOST \-U USER |
| cifs-utils | Mounts Windows/Samba shares as folders. | sudo mount \-t cifs //HOST/SHARE /mnt \-o username=USER |
| nfs-common | Mounts NFS shares. | showmount \-e HOST |
| filezilla | Graphical FTP and SFTP client. | Open from Activities |
| xrdp | RDP server, so other machines can remote into this desktop. | systemctl status xrdp |

## **Hardware and disks**

| Package | What it does | Try it |
| :---- | :---- | :---- |
| smartmontools | Drive SMART health data, to spot failing disks. | sudo smartctl \-H /dev/sda |
| nvme-cli | Manages NVMe SSDs; health and error logs. | sudo nvme smart-log /dev/nvme0 |
| lshw | Detailed list of all hardware. | sudo lshw \-short |
| hwinfo | Hardware report, good for drivers and device IDs. | hwinfo \--short |
| inxi | Quick, readable system summary (popular on forums). | inxi \-Fz |
| pciutils | lspci: lists PCI devices like NICs and GPUs. | lspci \-k |
| usbutils | lsusb: lists USB devices. | lsusb |
| dmidecode | BIOS info: model, serial, BIOS version, RAM slots. | sudo dmidecode \-t system |
| lm-sensors | Temperature, fan and voltage sensors. | sensors |
| memtester | Tests RAM for errors from inside Linux. | sudo memtester 512M 1 |
| stress-ng | Stress-tests CPU, memory and disks. | stress-ng \--cpu 4 \--timeout 60s |
| fio | Benchmarks disk speed and latency. | fio \--name=test \--rw=randread \--bs=4k \--size=256M |
| gparted | Graphical partition editor. | Open from Activities |
| testdisk | Recovers lost partitions; includes PhotoRec for deleted files. | sudo testdisk |
| gddrescue | Copies data off failing drives, skipping bad areas first. | sudo ddrescue /dev/sdX disk.img disk.map |
| exfatprogs | Formats and repairs exFAT drives. | sudo fsck.exfat /dev/sdX1 |
| ntfs-3g | Read and write support for NTFS drives. | sudo mount \-t ntfs-3g /dev/sdX1 /mnt |
| cryptsetup | Manages LUKS disk encryption. | sudo cryptsetup luksDump /dev/sdX2 |
| firmware-mediatek, firmware-realtek, firmware-amd-graphics | Firmware for MediaTek Wi-Fi/Bluetooth, Realtek network chips and AMD Radeon graphics. | sudo dmesg \| grep \-i firmware |

## **Admin and command line**

| Package | What it does | Try it |
| :---- | :---- | :---- |
| tmux | Split terminal panes; sessions survive disconnects. | tmux new \-s lab |
| htop | Interactive process viewer. | htop |
| btop | Visual system monitor with graphs. | btop |
| glances | All-in-one system monitor. | glances |
| iotop-c | Which processes are using the disk. | sudo iotop-c |
| ncdu | Interactive disk usage browser. | ncdu \~ |
| vim | Terminal text editor. | vimtutor |
| git | Version control: tracks changes, allows rollback. | git status |
| rsync | Efficient copy and sync, local or over SSH. | rsync \-avh SOURCE/ DEST/ |
| jq | Reads, filters and formats JSON. | jq . file.json |
| tree | Shows folders as a tree. | tree \-L 2 |
| lsof | Open files and ports and the processes holding them. | sudo lsof \-i :22 |
| strace | Shows a program's system calls, for debugging. | strace \-c ls |
| sysstat | sar, iostat, mpstat: performance stats. | iostat \-x 2 |
| plocate | Very fast file search by name. | locate FILENAME |
| unzip | Extracts .zip archives. | unzip FILE.zip |
| p7zip-full | 7z and many other archive formats. | 7z x FILE.7z |
| ansible | Configures many machines at once over SSH. | ansible localhost \-m ping |
| bash-completion | Tab completion for command options and arguments. | Type a command, then press Tab twice |
| ripgrep | Very fast text search across files. | rg PATTERN |
| fzf | Fuzzy finder. | Ctrl \+ R in the terminal |

## **Security**

| Package | What it does | Try it |
| :---- | :---- | :---- |
| ufw | Easy firewall front end. Installed but not enabled. | sudo ufw status verbose |
| lynis | Security audit that suggests hardening steps. | sudo lynis audit system |
| unattended-upgrades | Installs security updates automatically. | systemctl status unattended-upgrades |

## **Scripting and containers**

| Package | What it does | Try it |
| :---- | :---- | :---- |
| build-essential | Compilers and tools (gcc, make) to build from source. | gcc \--version |
| python3-pip | Python package installer. | pip3 \--version |
| python3-venv | Isolated Python environments. | python3 \-m venv .venv |
| pipx | Installs Python command-line apps in their own environments. | pipx list |
| docker.io | Runs apps in lightweight containers. | sudo docker run hello-world |
| docker-compose | Runs multi-container setups from one YAML file. | sudo docker-compose up \-d |

## **Desktop and apps**

| Package | What it does | Try it |
| :---- | :---- | :---- |
| task-gnome-desktop | The GNOME desktop and its standard apps. | Press Super |
| gnome-tweaks | Fonts, scaling, window buttons, startup apps. | gnome-tweaks |
| gnome-shell-extension-manager | Browse and manage GNOME extensions. | Open Extension Manager from Activities |
| flatpak | Sandboxed apps from Flathub. | flatpak search APPNAME |
| gnome-software-plugin-flatpak | Lets the Software app install Flatpak apps. | Open Software from the dock |
| libavcodec-extra, gstreamer1.0-libav | Extra audio and video codecs. | Nothing to run; more media formats just play |
| vlc | Plays almost any video or audio file. | vlc FILE |
| timeshift | System snapshots for rolling back after a bad update. | sudo timeshift \--list |
| brave-browser | Privacy-focused Chromium-based browser. | Open from the dock |
| tailscale | Mesh VPN built on WireGuard. | tailscale status |

