Perfect! Tum basically **Ansible ko use karke 5 servers pe Nginx install karna aur root ke bina deploy karna** chahte ho, aur **inventory ko custom location `/home/webserver` pe rakhna**. Main step-by-step full guide de raha hoon.

---

# 1️⃣ Ansible Installation (Ubuntu)

```bash id="a1"
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
ansible --version
```

---

# 2️⃣ Setup Non-Root User for Ansible

* Assume tumhare remote servers me ek user `webadmin` hai jo sudo without password use kar sakta hai.

```bash id="a2"
ssh webadmin@server1
sudo usermod -aG sudo webadmin
```

> Repeat for all 5 servers.

* Test sudo without password:

```bash id="a3"
ssh webadmin@server1
sudo -n whoami
```

Output should be `root`.

---

# 3️⃣ Inventory File Setup

* Tumhara custom inventory location: `/home/webserver/hosts`

```bash id="a4"
mkdir -p /home/webserver
nano /home/webserver/hosts
```

* Example inventory (5 servers):

```ini id="a5"
[webservers]
server1 ansible_host=192.168.1.101 ansible_user=webadmin
server2 ansible_host=192.168.1.102 ansible_user=webadmin
server3 ansible_host=192.168.1.103 ansible_user=webadmin
server4 ansible_host=192.168.1.104 ansible_user=webadmin
server5 ansible_host=192.168.1.105 ansible_user=webadmin
```

---

# 4️⃣ Ansible Config (`ansible.cfg`)

* Custom config file location: `/home/webserver/ansible.cfg`

```bash id="a6"
nano /home/webserver/ansible.cfg
```

* Minimal config:

```ini id="a7"
[defaults]
inventory = /home/webserver/hosts
remote_user = webadmin
ask_become_pass = false
host_key_checking = False
```

> `ask_become_pass = false` assumes sudo without password.

---

# 5️⃣ Test Ansible Connectivity

```bash id="a8"
ansible all -m ping
```

Expected output:

```text id="a9"
server1 | SUCCESS => {"changed": false, "ping": "pong"}
server2 | SUCCESS => {"changed": false, "ping": "pong"}
...
```

---

# 6️⃣ Create Playbook to Install Nginx

* File: `/home/webserver/nginx.yml`

```yaml id="b1"
---
- name: Install Nginx on all webservers
  hosts: webservers
  become: yes   # sudo
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Ensure Nginx is running
      service:
        name: nginx
        state: started
        enabled: yes
```

---

# 7️⃣ Run the Playbook

```bash id="b2"
cd /home/webserver
ansible-playbook nginx.yml
```

* Since `become: yes` aur `webadmin` user ka sudo enabled hai, root ke bina bhi Nginx install ho jayega.

---

# 8️⃣ Verify Installation

* Check Nginx status on remote servers:

```bash id="b3"
ansible all -a "systemctl status nginx"
```

* Or ping via browser:

```bash id="b4"
curl http://server1
```

---

# 🔹 Notes / Best Practices

1. **No root SSH**: Purely use `webadmin` + `become: yes`.
2. **Custom inventory**: `/home/webserver/hosts`
3. **Custom ansible.cfg**: `/home/webserver/ansible.cfg`
4. **Idempotency**: Playbook ko multiple times run kar sakte ho, Nginx duplicate install nahi hoga.
5. **SSH keys**: Make sure `webadmin` key is copied to all servers.

---
