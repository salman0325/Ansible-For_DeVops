
# 1️⃣ Inventory File with Groups

* Location: `/home/webserver/hosts` (custom)

```ini id="inv01"
[frontend]
frontend1 ansible_host=192.168.1.101 ansible_user=webadmin
frontend2 ansible_host=192.168.1.102 ansible_user=webadmin

[backend]
backend1 ansible_host=192.168.1.103 ansible_user=webadmin
backend2 ansible_host=192.168.1.104 ansible_user=webadmin

[database]
db1 ansible_host=192.168.1.105 ansible_user=webadmin

[all_servers:children]
frontend
backend
database
```

✅ Explanation:

* `[frontend]`, `[backend]`, `[database]` = logical groups
* `[all_servers:children]` = sab group ka super-group
* Isse tum **group-wise playbook** run kar sakte ho

---

# 2️⃣ Running Job on Specific Server

```bash id="cmd01"
ansible frontend1 -m ping
```

* Output:

```text
frontend1 | SUCCESS => {"ping": "pong"}
```

---

# 3️⃣ Run Job on a Group

```bash id="cmd02"
ansible frontend -m ping
```

* Only frontend servers ko run karega

---

# 4️⃣ Run Playbook on Single Server

```bash id="cmd03"
ansible-playbook nginx.yml --limit frontend1
```

* `--limit` ka use ek server ya ek group select karne ke liye

```bash id="cmd04"
ansible-playbook nginx.yml --limit backend
```

* Backend group pe deploy karega

---

# 5️⃣ Run Playbook Serially (Ek-ek server)

* Playbook me `serial` parameter use kar sakte ho:

```yaml id="py01"
- name: Install Nginx one by one
  hosts: frontend
  serial: 1  # Ek server pe ek time me run kare
  become: yes
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
```

* `serial: 1` = ek ek server pe deploy
* `serial: 2` = 2 servers ek sath

---

# 6️⃣ Make Inventory Permanent System-wide

1. Move inventory to system-wide location (recommended):

```bash id="sys01"
sudo mkdir -p /etc/ansible
sudo cp /home/webserver/hosts /etc/ansible/hosts
```

2. Set system environment variable:

```bash id="sys02"
echo 'export ANSIBLE_INVENTORY=/etc/ansible/hosts' | sudo tee /etc/profile.d/ansible_inventory.sh
sudo chmod +x /etc/profile.d/ansible_inventory.sh
source /etc/profile.d/ansible_inventory.sh
```

* Ab **system restart ke baad bhi** Ansible automatically ye inventory use karega

3. Optional: test

```bash id="sys03"
ansible all -m ping
```

---

# 7️⃣ Group-wise Playbook Example

`nginx.yml`:

```yaml id="py02"
---
- name: Install Nginx on Frontend
  hosts: frontend
  become: yes
  serial: 1
  tasks:
    - name: Update apt
      apt:
        update_cache: yes
    - name: Install Nginx
      apt:
        name: nginx
        state: present
    - name: Ensure Nginx running
      service:
        name: nginx
        state: started
        enabled: yes

- name: Install Backend Packages
  hosts: backend
  become: yes
  serial: 1
  tasks:
    - name: Update apt
      apt:
        update_cache: yes
    - name: Install python3
      apt:
        name: python3
        state: present

- name: Setup DB Server
  hosts: database
  become: yes
  tasks:
    - name: Install MySQL
      apt:
        name: mysql-server
        state: present
    - name: Ensure MySQL running
      service:
        name: mysql
        state: started
        enabled: yes
```

---

# 8️⃣ Key Commands Summary

| Command                                        | Purpose                |
| ---------------------------------------------- | ---------------------- |
| `ansible frontend1 -m ping`                    | Ping single server     |
| `ansible frontend -m ping`                     | Ping group             |
| `ansible-playbook nginx.yml --limit frontend1` | Playbook single server |
| `ansible-playbook nginx.yml --limit backend`   | Playbook group         |
| `serial: 1`                                    | Ek-ek server pe deploy |

---

# 🔹 Notes / Best Practices

1. **Groups** se tum easily environment divide kar sakte ho (frontend/backend/db)
2. **serial** parameter ek-ek server pe kaam karne ke liye
3. **Permanent inventory** via `$ANSIBLE_INVENTORY` environment variable
4. **Root SSH** unnecessary, sirf sudo-enabled user (`webadmin`) use karo
5. Multiple playbooks alag-alag groups ke liye banaye ja sakte hain

---

