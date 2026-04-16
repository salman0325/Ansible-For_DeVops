
# 🔹 1. Basic Variable (vars)

Yeh sab se simple hota hai:

```yaml
---
- hosts: all
  vars:
    package_name: nginx

  tasks:
    - name: install package
      apt:
        name: "{{ package_name }}"
        state: present
```

👉 **Use case:**

* Reusable code
* Same playbook multiple servers ke liye

---

# 🔹 2. Variable Types (Mostly Used)

### ✅ String

```yaml
name: "nginx"
```

### ✅ Number

```yaml
port: 80
```

### ✅ Boolean

```yaml
enabled: true
```

### ✅ List

```yaml
packages:
  - nginx
  - git
  - curl
```

### ✅ Dictionary

```yaml
user:
  name: salman
  shell: /bin/bash
```

---

# 🔹 3. Loop with Variables (VERY IMPORTANT 🔥)

```yaml
vars:
  packages:
    - nginx
    - git
    - curl

tasks:
  - name: install multiple packages
    apt:
      name: "{{ item }}"
      state: present
    loop: "{{ packages }}"
```

👉 **Use case:** bulk install, automation fast

---

# 🔹 4. Variable from Inventory

📄 `inventory.ini`

```
web ansible_host=192.168.1.10 http_port=80
```

📄 Playbook:

```yaml
- hosts: web
  tasks:
    - debug:
        msg: "Port is {{ http_port }}"
```

👉 **Use case:**

* Different server → different config

---

# 🔹 5. group_vars / host_vars (INDUSTRY STANDARD ⭐)

📁 Structure:

```
inventory
group_vars/
   web.yml
host_vars/
   server1.yml
```

📄 `group_vars/web.yml`

```yaml
package_name: nginx
```

👉 **Use case:**

* Production level projects
* Clean code
* Team collaboration

---

# 🔹 6. Registered Variables (Output store karna)

```yaml
tasks:
  - name: check disk
    command: df -h
    register: disk_output

  - debug:
      var: disk_output.stdout
```

👉 **Use case:**

* Output capture
* Decision making

---

# 🔹 7. Condition with Variables (WHEN 🔥)

```yaml
vars:
  install_nginx: true

tasks:
  - name: install nginx
    apt:
      name: nginx
      state: present
    when: install_nginx
```

👉 **Use case:**

* Conditional automation

---

# 🔹 8. Facts Variables (Auto system info)

```yaml
- debug:
    var: ansible_os_family
```

👉 Example:

* OS detect (Ubuntu / CentOS)
* RAM / CPU info

---

# 🔹 9. Command Line Variables (Extra Vars)

```bash
ansible-playbook play.yml -e "package_name=apache2"
```

👉 **Use case:**

* Runtime change
* Same playbook, different result

---

# 🔹 10. Default Variables (Roles me use hota hai)

📁 roles/web/defaults/main.yml

```yaml
package_name: nginx
```

👉 **Use case:**

* Flexible roles
* Override possible

---

# 🔹 11. Variable Precedence (IMPORTANT ⚠️)

Priority (top wins):

1. Extra vars (`-e`)
2. Task vars
3. Play vars
4. Inventory vars
5. Role defaults

👉 Interview me poocha jata hai 🔥

---

# 🔹 12. Real Industry Example (Full)

```yaml
---
- hosts: web
  become: yes

  vars:
    packages:
      - nginx
      - git

  tasks:
    - name: install packages
      apt:
        name: "{{ item }}"
        state: present
      loop: "{{ packages }}"

    - name: start nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

---

# 🔥 Real Use Cases Summary

| Use Case                | Variable Type  |
| ----------------------- | -------------- |
| Multiple servers config | inventory vars |
| Reusable code           | vars           |
| Dynamic output          | register       |
| Conditional deploy      | when           |
| Large projects          | group_vars     |
| Runtime change          | extra vars     |

---



# 🔹 1. Inventory me Variable define karna

📄 `inventory.ini`

```ini
[web]
server1 ansible_host=192.168.18.67 http_port=80 package_name=nginx
server2 ansible_host=192.168.18.68 http_port=8080 package_name=apache2
```

👉 Yahan:

* `http_port`
* `package_name`

yeh dono **inventory variables** hain

---

# 🔹 2. Playbook me use karna (VERY SIMPLE)

📄 `playbook.yml`

```yaml
---
- hosts: web
  become: yes

  tasks:
    - name: install package from inventory var
      apt:
        name: "{{ package_name }}"
        state: present

    - name: show port
      debug:
        msg: "Server is running on port {{ http_port }}"
```

👉 Yahan:

* `{{ package_name }}` → inventory se aa raha hai
* `{{ http_port }}` → inventory se aa raha hai

---

# 🔹 3. Run karna

```bash
ansible-playbook -i inventory.ini playbook.yml
```

---

# 🔥 Output samajh lo

| Server  | Package Install | Port |
| ------- | --------------- | ---- |
| server1 | nginx           | 80   |
| server2 | apache2         | 8080 |

👉 SAME playbook → DIFFERENT result 😎

---

# 🔹 4. Real Industry Use Case

### Example: Different servers pe different config

```ini
[web]
web1 ansible_host=192.168.1.10 package=nginx
web2 ansible_host=192.168.1.11 package=apache2

[db]
db1 ansible_host=192.168.1.20 package=mysql-server
```

📄 Playbook:

```yaml
- hosts: all
  become: yes

  tasks:
    - name: install package
      apt:
        name: "{{ package }}"
        state: present
```

👉 Auto:

* web → nginx/apache
* db → mysql

---

# 🔹 5. Agar variable na mile to error aata hai ⚠️

Fix:

```yaml
name: "{{ package_name | default('nginx') }}"
```

👉 Safe coding 🔥

---

# 🔹 6. Best Practice ⭐

Instead of `.ini`, use YAML inventory:

📄 `inventory.yml`

```yaml
all:
  children:
    web:
      hosts:
        server1:
          ansible_host: 192.168.18.67
          package_name: nginx
          http_port: 80
```

👉 Clean + production ready

---

# 🔹 7. Quick Summary

👉 Inventory me variable likho:

```ini
package_name=nginx
```

👉 Playbook me use karo:

```yaml
name: "{{ package_name }}"
```

👉 Bas itna hi simple hai 💯

---

