# journalctl-lab

```markdown
# 🔧 Journalctl Troubleshooting Lab  
*A Hands-On Linux Logging & Outage Response Tutorial by Noe Tovar-MBA*

[Linux](https://img.shields.io/badge/Linux-22%2B-blue?style=for-the-badge&logo=linux)
[Bash](https://img.shields.io/badge/Bash-Scripting-green?style=for-the-badge&logo=gnu-bash)
[WSL](https://img.shields.io/badge/WSL-Ubuntu-orange?style=for-the-badge&logo=microsoft)
[systemd](https://img.shields.io/badge/systemd-Logging-blue?style=for-the-badge&logo=systemd)
[Tutorial](https://img.shields.io/badge/Tutorial-Hands--On-red?style=for-the-badge)

---

##  Overview

This repository accompanies a hands-on tutorial designed to teach Linux users, sysadmins, DevOps engineers, and beginners how to troubleshoot system outages using:

- `journalctl`  
- systemd service logs  
- time-based log filtering  
- real-time log streaming  
- simulated service failures  
- safe testing using WSL on Windows  

I built this lab so you can safely practice real outage scenarios without touching a production server. You’ll create a systemd service, break it intentionally, and use `journalctl` to diagnose the problem exactly as a senior Linux engineer would.

---

## 🧪 Lab Contents

```

lab/
├── demo.service      # systemd service file
├── demo.sh           # simple script that generates log output
└── exercises.txt     # training tasks and troubleshooting drills

````

- `demo.service` — the systemd unit file you enable and start  
- `demo.sh` — the script that logs output every few seconds  
- `exercises.txt` — real-world troubleshooting challenges  

---

##  Getting Started (WSL + Ubuntu 24.04)

This lab works flawlessly using **Windows Subsystem for Linux (WSL)**.

### 1. Install WSL + Ubuntu  
Open PowerShell as Administrator:

```powershell
wsl --install -d Ubuntu
````

Reboot when prompted.

### 2. Update Ubuntu inside WSL

```bash
sudo apt update && sudo apt upgrade -y
```

### 3. Install essential tools

```bash
sudo apt install systemd-timesyncd git vim build-essential -y
```

### 4. Copy lab files to your system

Clone or download this repository:

```bash
git clone https://github.com/noetovar5/journalctl-lab.git
```

---

##  Create and Enable the Service

### Move the service files

```bash
sudo mkdir -p /opt/demo-service
sudo cp lab/demo.sh /opt/demo-service/
sudo chmod +x /opt/demo-service/demo.sh
sudo cp lab/demo.service /etc/systemd/system/
```

### Enable & start the service

```bash
sudo systemctl daemon-reload
sudo systemctl enable demo.service
sudo systemctl start demo.service
```

---

##  Viewing Service Logs

### View all logs for this service

```bash
sudo journalctl -u demo.service
```

### Tail logs live (real-time)

```bash
sudo journalctl -u demo.service -f
```

### Filter by time

```bash
sudo journalctl -u demo.service --since "10 minutes ago"
```

---

##  Break the Service (Practice Real Outages)

Rename the script:

```bash
sudo mv /opt/demo-service/demo.sh /opt/demo-service/broken.sh
sudo systemctl restart demo.service
```

Now check logs:

```bash
sudo journalctl -u demo.service -p err -n 20
```

---

##  Exercises

View the `exercises.txt` file for hands-on challenges such as:

* Finding errors for a unit within a time window
* Viewing logs from the previous boot
* Exporting logs to a file
* Filtering kernel-level errors

---

## Author

**Noe Tovar – MBA**
Systems Administrator | Linux Enthusiast | IT Trainer
[www.noetovar.com](http://www.noetovar.com)

---

##  License

This repository is licensed under the MIT License (see below).

````

---

# **LICENSE (MIT License)**  
Copy/paste this into a `LICENSE` file:

```markdown
MIT License

Copyright (c) 2025 Noe Tovar

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the “Software”), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
````

---

# 🗂 **.gitignore**

Add this `.gitignore` file to keep your repo clean:

```gitignore
# Ignore Python bytecode
__pycache__/
*.pyc

# Ignore system logs
*.log
*.journal

# macOS
.DS_Store

# Editors
.vscode/
.idea/

# Bash history
.bash_history

# Temporary files
tmp/
*.tmp

# Compiled binaries
*.out
*.o

# Backup files created by text editors
*~
*.swp
*.swo

# Node (in case of tooling)
node_modules/

# ZIPs or local export bundles
*.zip

