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

## 🔁 Permanent Mount

```bash
sudo nano /etc/fstab
```

Add:

```bash
192.168.1.10:/nfs_share /mnt/nfs nfs defaults 0 0
```

---

# 🔹 6. Common Ansible Commands

## 🔸 Ping

```bash
ansible all -m ping
```

## 🔸 Install Package

```bash
ansible all -m apt -a "name=nginx state=present" --become
```

## 🔸 Copy File

```bash
ansible all -m copy -a "src=file.txt dest=/tmp/"
```

## 🔸 Start Service

```bash
ansible all -m service -a "name=nginx state=started" --become
```

---


