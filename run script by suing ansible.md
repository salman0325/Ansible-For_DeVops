# 🔹 STEP 1: Shell Script banao (Practice Script)

📄 `script.sh`

```bash id="l7c2k1"
#!/bin/bash

echo "===== SYSTEM INFO ====="
echo "Hostname: $(hostname)"
echo "User: $(whoami)"
echo "Date: $(date)"

echo "===== MEMORY ====="
free -h

echo "===== DISK ====="
df -h

echo "===== DONE ====="
```

👉 Permission do:

```bash id="h8zx1y"
chmod +x script.sh
```

---

# 🔹 STEP 2: Inventory File

📄 `inventory.ini`

```ini id="d3kp9q"
[servers]
192.168.18.67
192.168.18.68
```

---

# 🔥 METHOD 1: Ad-hoc command (Direct run)

```bash id="o6m0zr"
ansible servers -i inventory.ini -m shell -a "/home/salman/script.sh"
```

👉 Agar sudo chahiye:

```bash id="nlfu3w"
ansible servers -i inventory.ini -m shell -a "/home/salman/script.sh" -b
```

---

# 🔥 METHOD 2: Playbook using `script` (BEST ⭐)

📄 `run_script.yml`

```yaml id="v6l0x4"
---
- hosts: servers
  become: yes

  tasks:
    - name: Run local script on remote servers
      script: /home/salman/script.sh
```

👉 Run:

```bash id="x2o8fv"
ansible-playbook -i inventory.ini run_script.yml
```

👉 Yeh karega:

* Script copy karega remote server pe
* Execute karega

---

# 🔥 METHOD 3: Playbook using `shell`

👉 Jab script already remote pe ho

📄 `run_remote.yml`

```yaml id="5a9s1k"
---
- hosts: servers
  become: yes

  tasks:
    - name: Run script from remote machine
      shell: /home/salman/script.sh
```

---

# 🔥 METHOD 4: Output capture (ADVANCED 🔥)

📄 `output.yml`

```yaml id="g4q9ws"
---
- hosts: servers

  tasks:
    - name: Run script and capture output
      shell: /home/salman/script.sh
      register: result

    - name: Show output
      debug:
        var: result.stdout
```

---

# 🔥 BONUS: Script with Argument (REAL USE 🔥)

📄 `script.sh`

```bash id="j0x3mz"
#!/bin/bash

NAME=$1

echo "Hello $NAME"
echo "Server: $(hostname)"
```

👉 Playbook:

```yaml id="k2t6rq"
- hosts: servers

  tasks:
    - name: run script with argument
      shell: /home/salman/script.sh Salman
```

---

# 🔥 REAL DEVOPS USE CASES

| Task                | Example            |
| ------------------- | ------------------ |
| Server health check | CPU, RAM script    |
| Deployment          | App restart script |
| Backup              | DB backup script   |
| Monitoring          | Logs collect       |

---

# 🔥 IMPORTANT DIFFERENCE (Yaad rakhna)

| Module  | Use                   |
| ------- | --------------------- |
| script  | Local → Remote run    |
| shell   | Remote command/script |
| command | Simple & secure       |

---

