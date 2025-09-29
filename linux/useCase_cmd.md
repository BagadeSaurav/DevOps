# 🔐 File Permissions & Ownership in Linux

Linux uses a **permission model** to control access to files and directories.  
Each file/directory has an **owner**, a **group**, and **permissions** for who can read, write, or execute it.  

---

## **1. File Ownership**
- **User (Owner)** → The person who created the file (or assigned owner).
- **Group** → A collection of users who share permissions.
- **Others** → All other users on the system.

### Check ownership:
```bash
ls -l
````

Example output:

```
-rwxr-xr--  1 alice  developers  1234 Sep 28 12:00 script.sh
```

Explanation:

* **alice** → Owner
* **developers** → Group
* **1234** → File size in bytes

---

## **2. File Permissions**

Permissions are shown as **rwx**:

* **r** = Read
* **w** = Write (modify)
* **x** = Execute (run as program/script)

They apply separately to:

* **User (u)** – file owner
* **Group (g)** – group members
* **Others (o)** – everyone else

Example:

```
-rwxr-xr--
```

* **Owner (u)** → `rwx` → read, write, execute
* **Group (g)** → `r-x` → read, execute
* **Others (o)** → `r--` → read only

---

## **3. Changing Ownership**

Change **file owner**:

```bash
sudo chown newuser filename
```

Change **file group**:

```bash
sudo chgrp newgroup filename
```

Change **both**:

```bash
sudo chown newuser:newgroup filename
```

---

## **4. Changing Permissions**

### Using `chmod` (symbolic mode):

```bash
chmod u+x script.sh   # Add execute permission to user
chmod g-w report.txt  # Remove write permission for group
chmod o+r file.txt    # Give read permission to others
```

### Using `chmod` (numeric mode):

Each permission has a value:

* **r = 4, w = 2, x = 1**

Add them up:

* `7 = rwx`
* `6 = rw-`
* `5 = r-x`
* `4 = r--`

Examples:

```bash
chmod 755 script.sh   # rwx for user, r-x for group & others
chmod 644 file.txt    # rw- for user, r-- for group & others
chmod 700 secret.txt  # rwx only for user
```

---

## **5. Use Cases**

### ✅ Protect Sensitive Files

```bash
chmod 600 id_rsa
```

* Only the owner can read/write (used for SSH private keys).

---

### ✅ Shared Project Directory

```bash
sudo chown -R :developers /project
chmod -R 770 /project
```

* Group `developers` has full access.
* Others have **no access**.

---

### ✅ Publicly Accessible File

```bash
chmod 644 index.html
```

* Owner can edit.
* Everyone can read (common for web servers).

---

### ✅ Executable Scripts

```bash
chmod +x backup.sh
./backup.sh
```

* Script becomes executable.

---

## **6. Special Permissions**

* **SUID (Set User ID)** → Run as file’s owner.
* **SGID (Set Group ID)** → Run as file’s group.
* **Sticky Bit** → On directories, only file owner can delete their files.

Examples:

```bash
chmod u+s program      # SUID
chmod g+s shared_dir   # SGID
chmod +t /tmp          # Sticky bit
```

----------------------------------------------------------------------------------------------------------------
# ⏰ Cron Jobs – Task Scheduling in Linux

## **1. What is a Cron Job?**
- A **cron job** is a scheduled task in Linux that runs **automatically** at specified times or intervals.  
- Managed by the **cron daemon (`crond`)**.  
- Commonly used for backups, log rotation, system maintenance, monitoring, etc.

---
### On Debian/Ubuntu:
```bash
sudo apt update
sudo apt install cron -y

## **2. Crontab Basics**
Each user can create/edit their own **crontab file**.

### View crontab
```bash
crontab -l
````

### Edit crontab

```bash
crontab -e
```

### Remove crontab

```bash
crontab -r
```

---

## **3. Crontab Syntax**

Format:

```
* * * * * command_to_execute
- - - - -
| | | | |
| | | | +---- Day of week (0–6) (Sunday=0)
| | | +------ Month (1–12)
| | +-------- Day of month (1–31)
| +---------- Hour (0–23)
+------------ Minute (0–59)
```

Examples:

* `0 5 * * * /home/user/backup.sh` → Run every day at **5:00 AM**.
* `*/10 * * * * /script.sh` → Run every **10 minutes**.
* `0 0 * * 0 /cleanup.sh` → Run every **Sunday at midnight**.

---

## **4. Use Cases for Cron Jobs**

### ✅ **Automated Backups**

```bash
0 2 * * * tar -czf /backup/home-$(date +\%F).tar.gz /home/user
```

* Runs daily at **2 AM**.
* Creates a compressed backup of `/home/user`.

---

### ✅ **Log Cleanup**

```bash
0 0 * * 7 find /var/log -type f -name "*.log" -delete
```

* Runs every **Sunday midnight**.
* Deletes old `.log` files.

---

### ✅ **System Updates**

```bash
0 3 * * 1 apt update && apt upgrade -y
```

* Runs every **Monday 3 AM**.
* Updates & upgrades packages.

---

### ✅ **Database Backup**

```bash
30 1 * * * mysqldump -u root -pMyPass mydb > /backup/mydb-$(date +\%F).sql
```

* Runs daily at **1:30 AM**.
* Dumps MySQL database into backup folder.

---

### ✅ **Custom Script Execution**

```bash
*/5 * * * * /home/user/scripts/monitor.sh
```

* Runs every **5 minutes**.
* Useful for monitoring, alerts, or automated tasks.

---

## **5. Special Keywords**

Instead of numbers, you can use keywords:

* `@reboot` → Run once at system startup.
* `@hourly` → Run every hour.
* `@daily` → Run once a day (midnight).
* `@weekly` → Run once a week.
* `@monthly` → Run once a month.
* `@yearly` → Run once a year.

Example:

```bash
@reboot /home/user/startup.sh
```

* Runs a script automatically after system reboot.

---

## **6. Cron Log Monitoring**

* Cron logs are usually stored in:

```bash
/var/log/cron
/var/log/syslog
```

To check logs:

```bash
grep CRON /var/log/syslog
```

-----------------------------------------------------------------------

# 📄 File Viewing & Editing in Linux

Linux provides multiple commands and editors to **view, read, and modify files**.  
These tools are essential for system admins, developers, and everyday users.  

---

## **1. Viewing Files**

### ✅ `less` – View file with navigation

```bash
less file.txt
```

* Scroll up/down with **↑ ↓ PgUp PgDn**.
* Quit with `q`.

---

### ✅ `more` – Page-by-page viewing

```bash
more file.txt
```

* Displays one screen at a time.
* Press **Space** to go forward.

---

### ✅ `head` – Show first lines

```bash
head -n 20 file.txt
```

* Shows the first 20 lines.

---

### ✅ `tail` – Show last lines

```bash
tail -n 50 logfile.log
```

* Shows last 50 lines.
* **Follow updates in real time**:

```bash
tail -f logfile.log
```

---

## **2. Editing Files**

### ✅ `nano` – Beginner-friendly text editor

```bash
nano file.txt
```

* Easy to use, shows shortcuts at bottom.
* Save: `CTRL+O`, Exit: `CTRL+X`.

---

### ✅ `vim` – Powerful text editor

```bash
vim file.txt
```

* Modes:

  * **Command mode** (default) → for navigation.
  * **Insert mode** → press `i` to edit.
  * **Save & quit** → `:wq`
  * **Quit without saving** → `:q!`

---

### ✅ `gedit` – GUI text editor

```bash
gedit file.txt
```

* Graphical editor for desktop Linux environments.

---

## **3. Use Cases**

### 📑 **Viewing Logs**

```bash
tail -f /var/log/syslog
```

* Continuously monitor system log in real time.

---

### 📝 **Quick Config File Edit**

```bash
nano filename
````

* Opens an existing file or creates a new one.
* Example:

```bash
nano notes.txt
```

---

## ** Basic Controls**

* `CTRL + O` → Save (Write Out)
* `CTRL + X` → Exit nano
* `CTRL + R` → Insert another file into current file
* `CTRL + W` → Search for text
* `CTRL + K` → Cut selected line
* `CTRL + U` → Paste (Uncut) line
* `CTRL + J` → Justify (format text)
* `CTRL + G` → Show help menu

👉 **Tip**: All commands are shown at the bottom of the editor screen.

---

## ** Editing Text**

* Start typing directly after opening the file.
* Use **Arrow Keys** to move around.
* To **insert a new line**, press `Enter`.

---

## ** Saving & Exiting**

* `CTRL + O` → Save changes

  * Press `Enter` to confirm filename.
* `CTRL + X` → Exit

  * If changes are unsaved, it will prompt:

    * `Y` → Save before exit
    * `N` → Exit without saving
    * `CTRL + C` → Cancel exit

---

## ** Searching & Replacing**

* `CTRL + W` → Search for text
* Type the keyword → Press `Enter`
* `CTRL + \` → Search & Replace

  * Enter search term
  * Enter replacement term
  * Confirm with `Y` or skip with `N`

---

## ** Cutting, Copying & Pasting**

* `CTRL + K` → Cut (removes current line)
* `CTRL + U` → Paste (uncut text)
* `ALT + 6` → Copy current line (without cutting)

---

## ** Navigation Shortcuts**

* `CTRL + A` → Move to beginning of line
* `CTRL + E` → Move to end of line
* `CTRL + Y` → Page up
* `CTRL + V` → Page down
* `CTRL + _` → Jump to a specific line number

---

## ** Common Use Cases**

✅ **Edit Config Files**

```bash
sudo nano /etc/hosts
```

✅ **Quick Script Editing**

```bash
nano deploy.sh
```


### ⚡ **Large File Analysis**

```bash
less /var/log/messages
```

* Navigate large log files without loading entire file into memory.

---

### 🛠️ **Monitoring Application Logs**

```bash
tail -n 100 -f /opt/app/logs/app.log
```

### **Viewing Files**
```bash
cat [options] [file...]
````

### **Common Options**

* `cat file.txt` → Display file content.
* `cat -n file.txt` → Show line numbers.
* `cat -b file.txt` → Show line numbers (non-blank only).
* `cat -s file.txt` → Squeeze multiple blank lines into one.

---

### **Use Cases**

✅ **View File Contents**

```bash
cat notes.txt
```

✅ **View Multiple Files Together**

```bash
cat file1.txt file2.txt
```

* Prints both files in sequence.

✅ **Create a New File**

```bash
cat > newfile.txt
```

(Type content → press `CTRL+D` to save & exit)

✅ **Append Data to a File**

```bash
cat >> existing.txt
```

(Add text → press `CTRL+D`)

✅ **Redirect Output to Another File**

```bash
cat file1.txt > file2.txt
```

* Overwrites `file2.txt` with `file1.txt`.

✅ **Combine Files**

```bash
cat file1.txt file2.txt > merged.txt
```

✅ **Numbered Display**

```bash
cat -n script.sh
```

* Displays line numbers.

---

---

## **2. `vim` Command (Vi IMproved Editor)**

`vim` is a **powerful text editor** that extends the classic `vi`.
It is widely used for editing **configuration files, scripts, and code**.

### **Launch vim**

```bash
vim filename
```

* Opens an existing file or creates a new one.

---

### **Modes in vim**

Vim operates in different modes:

1. **Normal Mode** (default)

   * For navigation & commands.
   * Press `Esc` to return here.

2. **Insert Mode**

   * For inserting/editing text.
   * Enter by pressing `i`, `a`, or `o`.

3. **Command Mode**

   * For saving, quitting, searching.
   * Access by pressing `:` in normal mode.

---

### **Basic Navigation**

* `h` → Move left
* `l` → Move right
* `j` → Move down
* `k` → Move up
* `:set number` → Show line numbers
* `:set nonumber` → Hide line numbers

---

### **Insert Text**

* `i` → Insert at cursor
* `a` → Insert after cursor
* `o` → Open new line below
* `O` → Open new line above

---

### **Save & Exit**

* `:w` → Save (write changes)
* `:q` → Quit
* `:wq` or `ZZ` → Save & quit
* `:q!` → Quit without saving

---

### **Editing & Deleting**

* `x` → Delete character
* `dd` → Delete line
* `yy` → Copy (yank) line
* `p` → Paste after cursor
* `u` → Undo
* `CTRL+r` → Redo

---

### **Searching**

* `/word` → Search for `word` forward
* `?word` → Search backward
* `n` → Repeat search in same direction
* `N` → Repeat search in opposite direction

---

### **Replacing Text**

* `:s/old/new/` → Replace first match in line
* `:s/old/new/g` → Replace all matches in line
* `:%s/old/new/g` → Replace all matches in file
* `:%s/old/new/gc` → Replace with confirmation

---

### **Multiple Files in vim**

Open multiple files:

```bash
vim file1 file2
```

Switch between files:

* `:n` → Next file
* `:prev` → Previous file
* `:ls` → List open files
* `:e filename` → Open another file

---

### **Split Windows**

* `:split file.txt` → Horizontal split
* `:vsplit file.txt` → Vertical split
* `CTRL+w w` → Switch between windows
* `CTRL+w q` → Close current split

---

### **Use Cases**

✅ **Edit Config File**

```bash
sudo vim /etc/hosts
```

✅ **Edit a Script**

```bash
vim deploy.sh
```

✅ **Search & Replace in Code**

```bash
:%s/localhost/127.0.0.1/g
```

✅ **Multi-file Editing**

```bash
vim *.txt
```
---

## **4. Quick Comparison of File Tools**

| Command/Editor | Usage                            |
| -------------- | -------------------------------- |
| `cat`          | Show small files quickly         |
| `less`         | Scroll through large files       |
| `head`         | See first few lines              |
| `tail`         | See last few lines (follow logs) |
| `nano`         | Easy command-line editing        |
| `vim`          | Advanced, powerful editing       |
| `gedit`        | Simple GUI editing               |

---

# 📦 Archiving & Compression in Linux

Archiving and compression are used to **bundle multiple files into one** and/or **reduce file size**.  
Common tools: `tar`, `gzip`, `bzip2`, `xz`, `zip`, `unzip`.

---

## **1. `tar` (Tape Archive)**

The `tar` command is mainly used for **archiving multiple files/folders into a single file** (with or without compression).

### **Syntax**
```bash
tar [options] archive_name.tar files...
````

### **Options**

* `-c` → Create archive
* `-x` → Extract archive
* `-v` → Verbose (show progress)
* `-f` → File name
* `-z` → Use gzip compression
* `-j` → Use bzip2 compression
* `-J` → Use xz compression

---

### **Examples**

✅ **Create an archive (no compression)**

```bash
tar -cvf backup.tar file1.txt file2.txt dir1/
```

✅ **Extract an archive**

```bash
tar -xvf backup.tar
```

✅ **Create a gzip-compressed archive**

```bash
tar -czvf backup.tar.gz file1.txt file2.txt dir1/
```

✅ **Extract a gzip-compressed archive**

```bash
tar -xzvf backup.tar.gz
```

✅ **Create a bzip2-compressed archive**

```bash
tar -cjvf backup.tar.bz2 file1.txt file2.txt
```

✅ **Extract a bzip2 archive**

```bash
tar -xjvf backup.tar.bz2
```

✅ **Create an xz-compressed archive**

```bash
tar -cJvf backup.tar.xz dir1/
```

✅ **Extract an xz archive**

```bash
tar -xJvf backup.tar.xz
```

✅ **List contents of a tar archive**

```bash
tar -tvf backup.tar
```

---

## **2. gzip / gunzip**

`gzip` compresses files, while `gunzip` extracts them.
It works only on **single files**, not directories.

✅ **Compress a file**

```bash
gzip file.txt
```

* Output → `file.txt.gz`

✅ **Decompress a file**

```bash
gunzip file.txt.gz
```

✅ **Keep original file while compressing**

```bash
gzip -k file.txt
```

---

## **3. bzip2 / bunzip2**

`bzip2` provides **better compression** than `gzip` but is slower.

✅ **Compress**

```bash
bzip2 file.txt
```

* Output → `file.txt.bz2`

✅ **Decompress**

```bash
bunzip2 file.txt.bz2
```

---

## **4. xz / unxz**

`xz` provides **highest compression ratio** (better than gzip/bzip2).

✅ **Compress**

```bash
xz file.txt
```

* Output → `file.txt.xz`

✅ **Decompress**

```bash
unxz file.txt.xz
```

✅ **Keep original file**

```bash
xz -k file.txt
```

---

## **5. zip & unzip**

Unlike `gzip`, `zip` can **compress multiple files/folders** into one archive.

✅ **Compress files into zip**

```bash
zip archive.zip file1.txt file2.txt
```

✅ **Compress a directory**

```bash
zip -r project.zip project_folder/
```

✅ **Extract a zip file**

```bash
unzip archive.zip
```

✅ **List contents without extracting**

```bash
unzip -l archive.zip
```

---

## **6. Real-World Use Cases**

✅ **Backup a project directory**

```bash
tar -czvf project_backup.tar.gz /home/user/project/
```

✅ **Extract a software package**

```bash
tar -xzvf software.tar.gz
```

✅ **Compress logs before sending**

```bash
gzip access.log
```

✅ **Share project as zip (cross-platform)**

```bash
zip -r project.zip /home/user/project/
```

---

## **Cheat Sheet**

| Tool    | Best For                                                                 |
| ------- | ------------------------------------------------------------------------ |
| `tar`   | Archiving multiple files into one (with/without compression).            |
| `gzip`  | Fast single-file compression.                                            |
| `bzip2` | Better compression ratio, slower.                                        |
| `xz`    | Best compression ratio, slowest.                                         |
| `zip`   | Widely used, compresses multiple files/directories (Windows compatible). |

---

# ⚙️ Process Management in Linux

A **process** is a running instance of a program.  
Linux provides multiple commands to **view, control, and manage processes**.

---

## **1. Viewing Processes**

### 🔹 `ps` – Process Status
Shows running processes for the current user/session.

**Examples**
```bash
ps                # List processes in the current shell
ps -e             # List all processes
ps -f             # Full format (UID, PID, PPID, start time, etc.)
ps -ef            # List all processes in full format
ps aux            # Show all processes (BSD style)
````

---

### 🔹 `top` – Real-Time Process Monitoring

Interactive tool to monitor CPU/memory usage of processes.

**Examples**

```bash
top               # Open real-time process view
top -u username   # Show processes of a specific user
```

Controls inside `top`:

* `q` → quit
* `k` → kill a process
* `h` → help
* `M` → sort by memory usage
* `P` → sort by CPU usage

---

### 🔹 `htop` – Improved `top` (if installed)

Colorful, interactive process viewer (easier than `top`).

```bash
htop
```

---

### 🔹 `pgrep` – Search for Processes

Finds PIDs by process name.

**Examples**

```bash
pgrep firefox      # Find PID(s) of firefox
pgrep -u user1     # List processes owned by a user
```

---

## **2. Managing Processes**

### 🔹 `kill` – Terminate Process by PID

Sends a signal to a process (default: `SIGTERM`).

**Examples**

```bash
kill 1234           # Kill process with PID 1234
kill -9 1234        # Force kill (SIGKILL)
kill -15 1234       # Graceful stop (SIGTERM)
```

---

### 🔹 `pkill` – Kill by Process Name

Kills processes by name instead of PID.

**Examples**

```bash
pkill firefox       # Kill all firefox processes
pkill -u user1      # Kill all processes of user1
```

---

### 🔹 `jobs` – List Background Jobs

Shows jobs started in the current terminal.

```bash
jobs
```

---

### 🔹 `fg` – Bring Job to Foreground

```bash
fg %1
```

(Brings job ID 1 to foreground)

---

### 🔹 `bg` – Resume Job in Background

```bash
bg %1
```

---

### 🔹 `nice` & `renice` – Process Priority

Processes have a **priority value (niceness)** from **-20 (highest priority) to 19 (lowest priority)**.

**Examples**

```bash
nice -n 10 ./script.sh   # Start script with lower priority
renice -n -5 -p 1234     # Change priority of PID 1234 to -5
```

---

## **3. Process States**

* **R** → Running
* **S** → Sleeping (waiting for event)
* **D** → Uninterruptible sleep (usually I/O)
* **T** → Stopped (paused)
* **Z** → Zombie (terminated but not cleaned)

---

## **4. Real-World Use Cases**

✅ **Find and kill a process using name**

```bash
pgrep -f "python app.py"
kill -9 <PID>
```

✅ **Check memory-hungry processes**

```bash
top -o %MEM
```

✅ **Run a process in the background**

```bash
./long_script.sh &
```

✅ **Check background jobs and bring one back**

```bash
jobs
fg %2
```

✅ **Start a program with low CPU priority**

```bash
nice -n 15 tar -czf backup.tar.gz /home/user/
```

---

## **Cheat Sheet**

| Command  | Purpose                             |
| -------- | ----------------------------------- |
| `ps`     | Show running processes              |
| `top`    | Real-time monitoring                |
| `htop`   | Interactive monitoring (better top) |
| `pgrep`  | Find PID by name                    |
| `kill`   | Kill by PID                         |
| `pkill`  | Kill by name                        |
| `jobs`   | Show background jobs                |
| `fg`     | Bring job to foreground             |
| `bg`     | Resume job in background            |
| `nice`   | Start with priority                 |
| `renice` | Change priority of running process  |

---
