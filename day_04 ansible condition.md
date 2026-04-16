# 🔥 1. Basic IF Condition (`when`)

```yaml id="y3b2s1"
- hosts: all
  vars:
    install_nginx: true

  tasks:
    - name: install nginx
      apt:
        name: nginx
        state: present
      when: install_nginx
```

👉 Matlab:

* agar `install_nginx = true` → task chalega
* warna skip ❌

---

# 🔥 2. IF-ELSE ka concept

Ansible me direct `else` nahi hota ❌
👉 hum 2 tasks likhte hain

```yaml id="9fcb4x"
vars:
  web_server: nginx

tasks:
  - name: install nginx
    apt:
      name: nginx
      state: present
    when: web_server == "nginx"

  - name: install apache
    apt:
      name: apache2
      state: present
    when: web_server == "apache"
```

👉 Yehi IF-ELSE hai 🔥

---

# 🔥 3. Multiple Conditions (AND / OR)

### ✅ AND

```yaml id="g1n6zd"
when: ansible_os_family == "Debian" and install_nginx
```

### ✅ OR

```yaml id="m1op8k"
when: ansible_os_family == "Debian" or ansible_os_family == "RedHat"
```

---

# 🔥 4. Loop + Condition (VERY IMPORTANT ⭐)

```yaml id="zqf0bh"
vars:
  packages:
    - nginx
    - git
    - apache2

tasks:
  - name: install only nginx
    apt:
      name: "{{ item }}"
      state: present
    loop: "{{ packages }}"
    when: item == "nginx"
```

👉 Sirf nginx install hoga 🔥

---

# 🔥 5. Condition using Registered Output

```yaml id="d6i2ax"
tasks:
  - name: check file exists
    command: ls /etc/nginx
    register: result
    ignore_errors: yes

  - name: install nginx if not exists
    apt:
      name: nginx
      state: present
    when: result.rc != 0
```

👉 Real use:

* Agar nginx nahi hai → install karo

---

# 🔥 6. Condition on OS (REAL INDUSTRY USE ⭐)

```yaml id="5k1j9o"
- name: install nginx on Ubuntu
  apt:
    name: nginx
    state: present
  when: ansible_os_family == "Debian"

- name: install nginx on CentOS
  yum:
    name: nginx
    state: present
  when: ansible_os_family == "RedHat"
```

👉 Multi-OS automation 🔥

---

# 🔥 7. Condition using Variables

```yaml id="n1b7zq"
vars:
  env: production

tasks:
  - name: run only in production
    debug:
      msg: "Production server"
    when: env == "production"
```

---

# 🔥 8. While loop ka alternative ❗

👉 Ansible me `while` nahi hota
👉 Instead use: `until` + retries

```yaml id="z1tk7k"
- name: wait for service
  shell: systemctl is-active nginx
  register: status
  until: status.stdout == "active"
  retries: 5
  delay: 5
```

👉 Matlab:

* 5 dafa try karega
* har 5 sec baad
* jab tak condition true na ho

---

# 🔥 9. Skip / Fail Conditions

### Skip example:

```yaml id="0n5t1y"
when: ansible_memtotal_mb > 512
```

### Fail example:

```yaml id="ax7v3x"
- name: fail if low memory
  fail:
    msg: "Not enough RAM"
  when: ansible_memtotal_mb < 512
```

---

# 🔥 10. Real Industry Example (FULL 🔥)

```yaml id="9np4zn"
---
- hosts: all
  become: yes

  vars:
    install_web: true

  tasks:

    - name: install nginx if enabled
      apt:
        name: nginx
        state: present
      when: install_web

    - name: start nginx only on Ubuntu
      service:
        name: nginx
        state: started
      when: ansible_os_family == "Debian"

    - name: check nginx status
      shell: systemctl is-active nginx
      register: nginx_status
      ignore_errors: yes

    - name: debug nginx status
      debug:
        var: nginx_status.stdout
```

---

# 🔥 Summary (Yaad rakhna 💯)

| Concept   | Ansible        |
| --------- | -------------- |
| if        | when           |
| else      | multiple tasks |
| while     | until          |
| loop      | loop           |
| condition | when           |

---

# 💡 Real DevOps Use Cases

| Situation             | Condition               |
| --------------------- | ----------------------- |
| OS based install      | when: ansible_os_family |
| Feature toggle        | when: variable          |
| Retry check           | until                   |
| Output based decision | register + when         |

---

# 💥 Pro Tip (Interview 🔥)

👉 Question: *Does Ansible support if-else?*
👉 Answer:

> Yes, using `when` condition. There is no direct else, but we use multiple tasks.

---

