
# 🔥 1️⃣ Important Ansible Files (short)

### 📄 1. `ansible.cfg`

👉 Settings file (Ansible ka behavior control)

### 📄 2. `hosts` (Inventory)

👉 Servers list (kin machines par run karna hai)

### 📄 3. Playbook (`.yml`)

👉 Automation script

```yaml
- hosts: web
  tasks:
    - name: install nginx
      apt:
        name: nginx
        state: present
```



# 📘 Ansible Modules, Commands & NFS Setup (Complete Guide)

---

# 🔹 1. Ansible Module Types

## ✅ 1. Command Module

* Simple Linux commands run karta hai
* Shell features (|, >, &&) support nahi karta

### 🔸 Examples:

```bash
ansible all -m command -a "uptime"
ansible all -m command -a "ls -l /home"
ansible all -m command -a "df -h"
```

---

## ✅ 2. Shell Module

* Command module jaisa hai but **shell features support karta hai**

### 🔸 Examples:

```bash
ansible all -m shell -a "ls -l | grep txt"
ansible all -m shell -a "echo hello > /tmp/test.txt"
ansible all -m shell -a "cat /etc/passwd | wc -l"
```

---

## ✅ 3. Fetch Module

👉 Remote server se file **local system (Ansible master)** par copy karta hai

### 🔸 Example:

```bash
ansible all -m fetch -a "src=/etc/hosts dest=/tmp/"
```

---

# 🔹 2. Module vs Command vs Shell (Difference)

| Feature      | Module        | Command           | Shell           |    |
| ------------ | ------------- | ----------------- | --------------- | -- |
| Type         | Built-in tool | Linux command run | Shell execution |    |
| Idempotent   | ✔️ Yes        | ❌ No              | ❌ No            |    |
| Pipes (      | )             | ❌                 | ❌               | ✔️ |
| Redirect (>) | ❌             | ❌                 | ✔️              |    |
| Safe         | ✔️ High       | ✔️ Medium         | ❌ Low           |    |
| Use Case     | Best practice | Simple command    | Complex command |    |

---

# 🧠 Easy Understanding

* **Module = Smart tool (recommended)**
* **Command = Simple instruction**
* **Shell = Advanced command (risky)**

👉 Rule:

* Module available ho → **module use karo**
* Simple command → **command**
* Complex command → **shell**

---

# 🔹 3. File Transfer Methods

## ✅ SCP (Secure Copy)

```bash
scp file.txt user@192.168.1.10:/home/user/
```

## ✅ RSYNC (Fast Sync)

```bash
rsync -avz file.txt user@192.168.1.10:/home/user/
```

### 🔥 Difference:

* SCP = simple copy
* RSYNC = fast + only changes transfer

---

# 🔹 4. NFS (Network File System)

👉 Ek server ka folder dusre servers pe share hota hai (central storage)

---

# 🔧 5. NFS Setup (Ubuntu Server)

## 🖥️ Step 1: Install NFS Server

```bash
sudo apt update
sudo apt install nfs-kernel-server -y
```

---

## 📁 Step 2: Create Shared Directory

```bash
sudo mkdir -p /nfs_share
sudo chown nobody:nogroup /nfs_share
sudo chmod 777 /nfs_share
```

---

## 📝 Step 3: Configure Export

```bash
sudo nano /etc/exports
```

Add:

```bash
/nfs_share 192.168.1.0/24(rw,sync,no_subtree_check)
```

---

## 🔄 Step 4: Restart Service

```bash
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

---

## 🖥️ Step 5: Client Setup

```bash
sudo apt install nfs-common -y
```

---

## 📂 Step 6: Mount NFS

```bash
sudo mkdir /mnt/nfs
sudo mount 192.168.1.10:/nfs_share /mnt/nfs
```





---

# 🔥 2️⃣ Most Important Modules (REAL USE)

Ab main tumhe **top modules + commands + options** deta hoon 👇

---

# 🔹 1. 📦 package module (apt/yum)

### ✅ Install package

```bash
ansible web -m apt -a "name=nginx state=present"
```

### ❌ Remove package

```bash
ansible web -m apt -a "name=nginx state=absent"
```

### 🔄 Update

```bash
ansible web -m apt -a "update_cache=yes"
```

👉 Important options:

* `name=` → package name
* `state=` → present / absent / latest

---

# 🔹 2. 📁 copy module

### File copy

```bash
ansible web -m copy -a "src=file.txt dest=/tmp/file.txt"
```

👉 Options:

* `src=` → local file
* `dest=` → remote path
* `mode=` → permission

---

# 🔹 3. 📂 file module

### Create directory

```bash
ansible web -m file -a "path=/data state=directory"
```

### Delete file

```bash
ansible web -m file -a "path=/data state=absent"
```

👉 Options:

* `path=`
* `state=` → file / directory / absent
* `mode=`

---

# 🔹 4. 🔍 find module

```bash
ansible web -m find -a "paths=/tmp patterns=*.log"
```

👉 Options:

* `paths=`
* `patterns=`
* `age=`

---

# 🔹 5. 👤 user module (VERY IMPORTANT 🔥)

### ✅ Create user

```bash
ansible web -m user -a "name=ali state=present"
```

### ❌ Delete user

```bash
ansible web -m user -a "name=ali state=absent"
```

### ➕ Add to group

```bash
ansible web -m user -a "name=ali groups=sudo append=yes"
```

👉 Options:

* `name=`
* `state=`
* `groups=`
* `append=yes`

---

# 🔹 6. 👥 group module

### Create group

```bash
ansible web -m group -a "name=dev state=present"
```

### Delete group

```bash
ansible web -m group -a "name=dev state=absent"
```

---

# 🔹 7. 🔧 service module

### Start service

```bash
ansible web -m service -a "name=nginx state=started"
```

### Stop

```bash
ansible web -m service -a "name=nginx state=stopped"
```

### Restart

```bash
ansible web -m service -a "name=nginx state=restarted"
```

### Enable on boot

```bash
ansible web -m service -a "name=nginx enabled=yes"
```

---

# 🔹 8. 📥 fetch module (reverse copy)

```bash
ansible web -m fetch -a "src=/var/log/syslog dest=./backup/"
```

👉 Remote → Local copy

---

# 🔹 9. ⚡ command module

```bash
ansible web -m command -a "uptime"
```

👉 Simple command run (no shell features)

---

# 🔹 10. 💻 shell module

```bash
ansible web -m shell -a "echo hello > /tmp/test.txt"
```

👉 Use when:

* pipes |
* redirects >

---

# 🔹 11. 🔗 ping module

```bash
ansible web -m ping
```

👉 Check connectivity

---

# 🔥 3️⃣ Most Used Flags (IMPORTANT)

| Flag       | Meaning        |
| ---------- | -------------- |
| `-m`       | module         |
| `-a`       | arguments      |
| `-i`       | inventory file |
| `-u`       | user           |
| `--become` | sudo           |
| `-k`       | ask password   |

---

### Example:

```bash
ansible web -m apt -a "name=nginx state=present" -u ubuntu --become
```

---

# 🔥 4️⃣ Real DevOps Flow (IMPORTANT 🚀)

1. Inventory (`hosts`)
2. Config (`ansible.cfg`)
3. Test (`ping`)
4. Run module / playbook

---

# 🔥 FINAL SHORT SUMMARY

👉 Important modules:

* package (apt/yum)
* copy
* file
* find
* user 👤
* group 👥
* service 🔧
* fetch
* command
* shell

👉 Sab se zyada use:
✔ user
✔ package
✔ service
✔ copy

