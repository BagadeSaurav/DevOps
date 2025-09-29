Linux Commands

 1File & Directory Management

| Command    | Description                                    | Example                           |
| ---------- | ---------------------------------------------- | --------------------------------- |
| `pwd`      | Show current directory path                    | `pwd`                             |
| `ls`       | List files in directory                        | `ls -l`                           |
| `cd`       | Change directory                               | `cd /etc`                         |
| `touch`    | Create empty file                              | `touch file.txt`                  |
| `mkdir`    | Create directory                               | `mkdir myfolder`                  |
| `cp`       | Copy files/directories                         | `cp file.txt backup.txt`          |
| cp -r      | copy folder                                    | cp -r cloud/  home/ 
| `mv`       | Move or rename files/directories               | `mv old.txt new.txt`              |
| `rm`       | Delete file                                    | `rm file.txt`                     |
| `rmdir`    | Remove empty directory                         | `rmdir olddir`                    |
| `tree`     | Show directory tree (may need install)         | `tree /etc`                       |
| `find`     | Search for files/directories                   | `find /etc -name hosts`           |
| `locate`   | Quickly find files (needs `mlocate`/`plocate`) | `locate passwd`                   |
| `stat`     | Show detailed file information                 | `stat file.txt`                   |
| `file`     | Show file type                                 | `file file.txt`                   |
| `more`     | View file page by page                         | `more file.txt`                   |
| `wc`       | Count lines, words, characters in file         | `wc -l file.txt`                  |
| `du`       | Show directory size usage                      | `du -sh /var/log`                 |
| `df`       | Show disk space usage                          | `df -h`                           |
| `basename` | Extract filename from path                     | `basename /etc/passwd` → `passwd` |
| `dirname`  | Extract directory name from path               | `dirname /etc/passwd` → `/etc`    |
| `realpath` | Show absolute path of file                     | `realpath file.txt`               |



2. File Viewing & Editing

| Command | Description                 | Example            |
| ------- | --------------------------- | ------------------ |
| `cat`   | View file contents          | `cat > file.txt`   |
| `less`  | Scroll through file         | `less /etc/passwd` |
| `head`  | Show first 10 lines         | `head file.txt`    |
| `tail`  | Show last 10 lines          | `tail file.txt`    |
| `nano`  | Edit file (simple editor)   | `nano file.txt`    |
| `vim`   | Edit file (advanced editor) | `vim file.txt`     |


3. File Permissions & Ownership

| Command | Description                                    | Example                          |
| ------- | ---------------------------------------------- | -------------------------------- |
| `ls -l` | Show file permissions & ownership              | `ls -l`                          |
|  ls -a  | showing hiden file folder                      | ls -a                            |
| `chmod` | Change file/directory permissions              | `chmod 755 script.sh`            |
| `chown` | Change file owner and group                    | `sudo chown user:group file.txt` |
| `chgrp` | Change group ownership only                    | `sudo chgrp developers file.txt` |
| `umask` | Show/set default permission mask for new files | `umask 022`                      |
| `stat`  | Show detailed file metadata (incl. perms)      | `stat file.txt`                  |
| `id`    | Show current user UID/GID & groups             | `id`                             |


4. File Search & Text Processing

| Command  | Description              | Example                      |
| -------- | ------------------------ | ---------------------------- |
| `find`   | Search for files         | `find / -name file.txt`      |
| `locate` | Fast file search         | `locate file.txt`            |
| `grep`   | Search text in files     | `grep "error" log.txt`       |
| `wc`     | Count words, lines       | `wc -l file.txt`             |
| `sort`   | Sort lines               | `sort names.txt`             |
| `uniq`   | Remove duplicates        | `uniq names.txt`             |
| `cut`    | Extract columns          | `cut -d, -f1 file.csv`       |
| `awk`    | Advanced text processing | `awk '{print $1}' file.txt`  |
| `sed`    | Stream editing           | `sed 's/old/new/g' file.txt` |


5. User & Group Management
   
| Command               | Description                        | Example                                                                |
| --------------------- | ---------------------------------- | ---------------------------------------------------------------------- |
| `whoami`              | Show current logged-in user        | `whoami` → `john`                                                      |
| `id`                  | Show UID, GID, and group info      | `id john` → `uid=1001(john) gid=1001(john) groups=1001(john),27(sudo)` |
| `adduser` / `useradd` | Create a new user                  | `sudo adduser alice`                                                   |
| `passwd`              | Set/Change user password           | `sudo passwd alice`                                                    |
| `usermod`             | Modify user account (group, shell) | `sudo usermod -aG sudo alice`                                          |
| `deluser` / `userdel` | Delete a user account              | `sudo deluser alice`                                                   |
| `groups`              | Show groups a user belongs to      | `groups alice` → `alice : alice sudo`                                  |
| `groupadd`            | Create a new group                 | `sudo groupadd developers`                                             |
| `groupdel`            | Delete a group                     | `sudo groupdel developers`                                             |
| `gpasswd`             | Add/Remove user from group         | `sudo gpasswd -d alice developers`                                     |
| `su -`                | Switch to another user             | `su - alice`                                                           |
|cd /etc   nano passwd  | view the all user and group in this| cd /etc     nano passwd                                                |

6. Process Management

| Command     | Description                                  | Example                  |
| ----------- | -------------------------------------------- | ------------------------ |
| `ps`        | List processes                               | `ps aux`                 |
| `top`       | Real-time process monitoring                 | `top`                    |
| `htop`      | Interactive process manager (needs install)  | `htop`                   |
| `pgrep`     | Find process ID by name                      | `pgrep firefox`          |
| `pidof`     | Find PID of a running program                | `pidof sshd`             |
| `kill`      | Kill process by PID                          | `kill -9 1234`           |
| `killall`   | Kill all processes by name                   | `killall firefox`        |
| `jobs`      | List background jobs in current shell        | `jobs`                   |
| `fg`        | Bring background job to foreground           | `fg %1`                  |
| `bg`        | Resume a stopped job in background           | `bg %1`                  |
| `nice`      | Start a process with priority                | `nice -n 10 myscript.sh` |
| `renice`    | Change priority of running process           | `renice -n 5 -p 1234`    |
| `nohup`     | Run process immune to hangups (logs to file) | `nohup python app.py &`  |
| `disown`    | Remove job from shell’s job table            | `disown %1`              |
| `systemctl` | Manage system services (systemd)             | `systemctl restart ssh`  |
| `service`   | Manage services (SysV init)                  | `service ssh restart`    |



7. Disk & Storage

| Command   | Description                                | Example                     |
| --------- | ------------------------------------------ | --------------------------- |
| `df`      | Show disk usage of mounted filesystems     | `df -h`                     |
| `du`      | Show directory/file size                   | `du -sh /var/log`           |
| `mount`   | Mount a filesystem/device                  | `sudo mount /dev/sdb1 /mnt` |
| `umount`  | Unmount a filesystem/device                | `sudo umount /mnt`          |
| `lsblk`   | List block devices and mount points        | `lsblk`                     |
| `blkid`   | Show UUIDs and filesystem types of devices | `blkid`                     |
| `free`    | Show RAM & swap memory usage               | `free -h`                   |
| `swapon`  | Enable swap space                          | `sudo swapon /swapfile`     |
| `swapoff` | Disable swap space                         | `sudo swapoff /swapfile`    |
| `fdisk`   | Manage disk partitions (interactive)       | `sudo fdisk /dev/sdb`       |
| `parted`  | Manage partitions (advanced, interactive)  | `sudo parted /dev/sdb`      |
| `mkfs`    | Create filesystem on partition/device      | `sudo mkfs.ext4 /dev/sdb1`  |
| `tune2fs` | Tune/view ext filesystem parameters        | `sudo tune2fs -l /dev/sda1` |
| `e2fsck`  | Check & repair ext2/ext3/ext4 filesystem   | `sudo e2fsck -f /dev/sda1`  |
| `df -i`   | Show inode usage                           | `df -i`                     |
| `ls -lh`  | Show file sizes in human-readable format   | `ls -lh /var/log/`          |

8. Networking

| Command         | Description                                  | Example                               |
| --------------- | -------------------------------------------- | ------------------------------------- |
| `ip addr`       | Show IP addresses                            | `ip addr show`                        |
| `ip route`      | Show routing table                           | `ip route show`                       |
| `ping`          | Test network connectivity                    | `ping google.com`                     |
| `traceroute`    | Show route packets take (needs install)      | `traceroute google.com`               |
| `dig`           | DNS lookup (needs `dnsutils`)                | `dig example.com`                     |
| `nslookup`      | DNS query (alternative to `dig`)             | `nslookup example.com`                |
| `curl`          | Fetch data from URL                          | `curl https://example.com`            |
| `wget`          | Download file from URL                       | `wget https://example.com/file.zip`   |
| `ss`            | Show sockets / listening ports               | `ss -tulpn`                           |
| `netstat`       | Legacy sockets/ports view (deprecated)       | `netstat -tulpn`                      |
| `scp`           | Copy files over SSH                          | `scp file.txt user@host:/path`        |
| `sftp`          | Interactive secure file transfer             | `sftp user@host`                      |
| `rsync`         | Sync/copy files over SSH                     | `rsync -av file.txt user@host:/path/` |
| `nc` / `netcat` | Simple networking tool (port scan, transfer) | `nc -zv host 22`                      |
| `ifconfig`      | Show/manage IP (old tool, replaced by `ip`)  | `ifconfig eth0`                       |
| mtr             | show ip details other u search               |  mtr google.com                       |
| telnet          | using port no                                | telnet google.com 80                  | 
|hostname         |host name configer                            |cat /etc/hosts                         |
| dig             | view full details                            | dig google.com                        |
| whois           | where is parchas and all                     |whois google.com                       |

9. Archiving & Compression

| Command   | Description                                  | Example                                        |
| --------- | -------------------------------------------- | ---------------------------------------------- |
| `tar`     | Create/extract archives                      | `tar -cvf archive.tar file1 file2`             |
|           | Extract archive                              | `tar -xvf archive.tar`                         |
|           | Create compressed tar (gzip)                 | `tar -czvf archive.tar.gz dir/`                |
|           | Extract compressed tar                       | `tar -xzvf archive.tar.gz`                     |
| `gzip`    | Compress file (replaces original)            | `gzip file.txt` → `file.txt.gz`                |
| `gunzip`  | Decompress `.gz` file                        | `gunzip file.txt.gz`                           |
| `zcat`    | View contents of `.gz` file                  | `zcat file.txt.gz`                             |
| `zip`     | Create `.zip` archive                        | `zip archive.zip file1 file2`                  |
| `unzip`   | Extract `.zip` archive                       | `unzip archive.zip`                            |
| `bzip2`   | Compress using bzip2 algorithm               | `bzip2 file.txt` → `file.txt.bz2`              |
| `bunzip2` | Decompress `.bz2` file                       | `bunzip2 file.txt.bz2`                         |
| `xz`      | Compress using xz algorithm                  | `xz file.txt` → `file.txt.xz`                  |
| `unxz`    | Decompress `.xz` file                        | `unxz file.txt.xz`                             |
| `7z`      | Create/extract `.7z` archive (needs `p7zip`) | `7z a archive.7z file.txt` / `7z x archive.7z` |

exmaple
    1  du -sh etc           #view of file size 
    2  tar -cvf /home/etc.tar /etc     #creating tar file
    3  cd /home/            #view file
    4  ls
    5  etc.tar 

10. System Services (systemd)

| Command             | Description     | Example                   |
| ------------------- | --------------- | ------------------------- |
| `systemctl status`  | Service status  | `systemctl status ssh`    |
| `systemctl start`   | Start service   | `systemctl start nginx`   |
| `systemctl stop`    | Stop service    | `systemctl stop nginx`    |
| `systemctl enable`  | Enable on boot  | `systemctl enable nginx`  |
| `systemctl disable` | Disable service | `systemctl disable nginx` |


12. Permissions & Links

| Command    | Description       | Example                           |
| ---------- | ----------------- | --------------------------------- |
| `ln`       | Create hard link  | `ln file1.txt file2.txt`          |
| `ln -s`    | Create soft link  | `ln -s /var/log/syslog mylog`     |
| `stat`     | Show link details | `stat file1.txt file2.txt mylog`  |
| `ls -i`    | Show inode number | `ls -i file1.txt file2.txt mylog` |
| `readlink` | Show symlink path | `readlink mylog`                  |


13. System Monitoring & Logs

| Command      | Description      | Example             |        |
| ------------ | ---------------- | ------------------- | ------ |
| `uptime`     | Show uptime      | `uptime`            |        |
| `free`       | Show memory      | `free -h`           |        |
| `dmesg`      | Kernel messages  | \`dmesg             | tail\` |
| `journalctl` | View system logs | `journalctl -u ssh` |        |
| `last`       | Login history    | `last`              |        |

14. Advanced / Security

| Command    | Description      | Example             |
| ---------- | ---------------- | ------------------- |
| `ssh`      | Remote login     | `ssh user@host`     |
| `ufw`      | Firewall control | `ufw allow 22`      |
| `iptables` | Packet filtering | `iptables -L`       |
| `cron`     | Scheduled tasks  | `crontab -e`        |
| `alias`    | Create shortcut  | `alias ll='ls -la'` |


15. System Information & Hardware

| Command     | Description                      |
| ----------- | -------------------------------- |
| `uname -a`  | Show kernel & OS info            |
| `hostname`  | Show system hostname             |
| `uptime`    | Show system uptime               |
| `lscpu`     | Show CPU info                    |
| `lsmem`     | Show memory block info           |
| `lsusb`     | List USB devices                 |
| `lspci`     | List PCI devices                 |
| `dmidecode` | Show hardware info from BIOS     |
| `inxi`      | Full system info (needs install) |

---

16. Memory & Performance Analysis

| Command   | Description                                             |
| --------- | ------------------------------------------------------- |
| `vmstat`  | Show memory, CPU stats                                  |
| `sar`     | Historical performance stats                            |
| `iostat`  | Disk I/O stats                                          |
| `iotop`   | Real-time disk usage per process                        |
| `free -m` | Memory usage                                            |
| `watch`   | Repeat command every few seconds (`watch -n 1 free -m`) |


---

17. Package Management (Extra)

* **Debian/Ubuntu**:

  * `dpkg -l` → List installed packages
  * `dpkg -i package.deb` → Install `.deb`
* **RHEL/Fedora**:

  * `rpm -qa` → List installed packages
  * `rpm -ivh package.rpm` → Install `.rpm`

---


18. System Shutdown & Reboot

| Command           | Description          |
| ----------------- | -------------------- |
| `shutdown -h now` | Shutdown immediately |
| `shutdown -r now` | Reboot immediately   |
| `reboot`          | Restart              |
| `halt`            | Halt system          |

---

19. Scripting & Development

| Command   | Description                          |
| --------- | ------------------------------------ |
| `which`   | Show location of command             |
| `type`    | Show if command is builtin or binary |
| `history` | Show command history                 |
| `source`  | Run commands from file               |
| `export`  | Set environment variables            |

---

20. Permissions & Ownership (Extra)

| Command   | Description             |
| --------- | ----------------------- |
| `stat`    | Show detailed file info |
| `getfacl` | Get ACL permissions     |
| `setfacl` | Set ACL permissions     |

---

21. Advanced Security

| Command     | Description                          |
| ----------- | ------------------------------------ |
| `who`       | Show logged-in users                 |
| `w`         | Show who is logged in & what they do |
| `last`      | Show login history                   |
| `lastlog`   | Show last login for all users        |
| `faillog`   | Show failed logins                   |
| `passwd -l` | Lock user account                    |
| `passwd -u` | Unlock user account                  |

---

# 🗂️ Linux Directory Structure Overview

Linux uses a **hierarchical directory structure** (tree-like) with `/` (root) at the top.  
Every file and directory starts from the **root directory**.

# View file structure

```bash
apt install tree
````
---

## **1. / (Root)**
- **Description**: The top-level directory in Linux.
- **Contains**: All other directories and system files.
- **Example**:
```bash
ls /
````

* Output: `bin boot dev etc home lib media mnt opt proc root run sbin srv tmp usr var`

---

## **2. /bin** – Essential User Binaries

* Contains **essential commands** used by all users.
* Examples: `ls`, `cp`, `mv`, `cat`, `bash`

---

## **3. /sbin** – System Binaries

* Contains **administrative commands** used by the root user.
* Examples: `ifconfig`, `iptables`, `shutdown`, `fdisk`

---

## **4. /etc** – Configuration Files

* System-wide **configuration files**.
* Examples: `passwd`, `hosts`, `fstab`, `ssh/sshd_config`
* Typically **not executable**.

---

## **5. /home** – User Home Directories

* Contains **personal directories** for users.
* Example:

```bash
/home/alice
/home/bob
```

---

## **6. /root** – Root User Home

* **Home directory** of the `root` user.
* Not to be confused with `/` (root of filesystem).

---

## **7. /lib & /lib64** – Shared Libraries

* Contains **shared libraries** required by system programs.
* Examples: `/lib/libc.so.6`, `/lib64/ld-linux-x86-64.so.2`

---

## **8. /usr** – User Programs & Data

* Contains **installed applications and files** for users.
* Subdirectories:

  * `/usr/bin` → Non-essential user commands
  * `/usr/sbin` → Non-essential system commands
  * `/usr/lib` → Libraries
  * `/usr/share` → Shared data (docs, man pages, icons)

---

## **9. /var** – Variable Data

* Contains **files that change frequently**:

  * Logs → `/var/log/`
  * Spools → `/var/spool/`
  * Databases → `/var/lib/`
* Example:

```bash
tail -f /var/log/syslog
```

---

## **10. /tmp** – Temporary Files

* Stores **temporary files** created by users or applications.
* Often cleared on reboot.

---

## **11. /boot** – Boot Files

* Contains **kernel and bootloader files**.
* Examples: `vmlinuz`, `initrd.img`, `grub/`

---

## **12. /dev** – Device Files

* Contains **device nodes** representing hardware.
* Examples: `/dev/sda`, `/dev/tty`, `/dev/null`

---

## **13. /proc** – Virtual Filesystem for Processes

* Provides **runtime system information**.
* Examples: `/proc/cpuinfo`, `/proc/meminfo`, `/proc/<pid>/`

---

## **14. /sys** – Kernel Information

* Virtual filesystem exposing **kernel-related info**.
* Example:

```bash
ls /sys/class/net
```

* Shows network interfaces.

---

## **15. /media & /mnt** – Mount Points

* `/media` → Removable media (USB, CD-ROM)
* `/mnt` → Temporary mount points for manually mounted devices

---

## **16. /opt** – Optional Software

* For **third-party applications**.
* Examples: `/opt/google`, `/opt/eclipse`

---

## **17. /srv** – Service Data

* Stores **data for services** like web or FTP.
* Example: `/srv/www`, `/srv/ftp`

---

## **18. /run** – Runtime Data

* Stores **temporary runtime files** (PIDs, sockets).
* Cleared on reboot.

---

## **Quick Directory Cheat Sheet**

| Directory         | Purpose                                  |
| ----------------- | ---------------------------------------- |
| `/`               | Root of filesystem                       |
| `/bin`            | Essential user binaries                  |
| `/sbin`           | System binaries (root-only)              |
| `/etc`            | Configuration files                      |
| `/home`           | User home directories                    |
| `/root`           | Root user home directory                 |
| `/lib` & `/lib64` | Shared libraries                         |
| `/usr`            | User programs & libraries                |
| `/var`            | Variable files (logs, spools, databases) |
| `/tmp`            | Temporary files                          |
| `/boot`           | Bootloader & kernel files                |
| `/dev`            | Device files                             |
| `/proc`           | Process & system info                    |
| `/sys`            | Kernel info                              |
| `/media`          | Removable media mount points             |
| `/mnt`            | Temporary mount points                   |
| `/opt`            | Optional software                        |
| `/srv`            | Service data                             |
| `/run`            | Runtime files                            |

---
