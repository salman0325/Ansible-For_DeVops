
# 📘 Ansible Playbook Examples 

---

# 🚀 1. Basic Playbook Examples (Single Tasks)

## 📌 Example 1: Install Package + Create File + Copy File

```yaml
---
# Playbook start: all servers target honge
- hosts: all              # target inventory group (all servers)
  become: yes             # root privileges enable

  tasks:                  # tasks section start

    - name: install git package   # task description
      apt:                        # Ubuntu/Debian package manager
        name: git                 # package name
        state: present            # ensure installed

    - name: copy passwd file      # file copy task
      copy:
        src: /etc/passwd          # source file on controller
        dest: /tmp/passwd         # destination on remote server

    - name: create empty file     # file creation task
      file:
        path: /tmp/ansible.txt    # file path
        state: touch              # create empty file
```

---

## 📌 Example 2: Add Content to File

```yaml
---
- hosts: all
  become: yes

  tasks:

    - name: write content to file   # add text inside file
      copy:
        content: "Hello Ansible World"  # text content
        dest: /tmp/hello.txt           # target file
```

---

## 📌 Example 3: Install Multiple Packages

```yaml
---
- hosts: all
  become: yes

  tasks:

    - name: install multiple packages
      apt:
        name:
          - git
          - curl
          - wget
        state: present
        update_cache: yes
```

---

# 🧩 2. Multi-Play Playbook (Web + App Servers)

## 📌 Example 1: Web + App Separation

```yaml
---
# WEB SERVER PLAY
- name: web server setup
  hosts: web              # web group servers
  become: yes

  tasks:

    - name: remove apache/nfs if exists
      apt:
        name: nfs-kernel-server
        state: absent      # uninstall package
        update_cache: yes

---

# APP SERVER PLAY
- name: app server setup
  hosts: app
  become: yes

  tasks:

    - name: install nginx
      apt:
        name: nginx
        state: present

    - name: start nginx service
      service:
        name: nginx
        state: started
        enabled: yes
```

---

## 📌 Example 2: Install Apache + Start Service

```yaml
---
- name: apache setup
  hosts: web
  become: yes

  tasks:

    - name: install apache2
      apt:
        name: apache2
        state: present

    - name: start apache service
      service:
        name: apache2
        state: started
        enabled: yes
```

---

## 📌 Example 3: Clean + Install Setup

```yaml
---
- name: clean and install setup
  hosts: all
  become: yes

  tasks:

    - name: remove old nginx
      apt:
        name: nginx
        state: absent

    - name: install fresh nginx
      apt:
        name: nginx
        state: present
```

---

# ⚙️ 3. Variables (Real Industry Use)

## 📌 Example 1: Basic Variable Usage

```yaml
---
- hosts: all
  become: yes

  vars:                     # variables block
    pkg: nginx             # package variable
    src: /home/site
    dest: /var/www/html

  tasks:

    - name: install package using variable
      apt:
        name: "{{ pkg }}"   # variable usage
        state: present

    - name: copy website files
      copy:
        src: "{{ src }}"
        dest: "{{ dest }}"
```

---

## 📌 Example 2: Service + Variable

```yaml
---
- hosts: all
  become: yes

  vars:
    service_name: nginx

  tasks:

    - name: install service
      apt:
        name: "{{ service_name }}"
        state: present

    - name: start service
      service:
        name: "{{ service_name }}"
        state: started
```

---

## 📌 Example 3: Variable with multiple packages

```yaml
---
- hosts: all
  become: yes

  vars:
    packages:
      - git
      - curl
      - nginx

  tasks:

    - name: install packages
      apt:
        name: "{{ packages }}"
        state: present
```

---

# 📂 4. Import Tasks (Modular Playbooks)

## 📌 Example 1: Import External Task File

```yaml
---
- name: main playbook
  hosts: all

  tasks:

    - name: import external tasks
      import_tasks: /home/user/tasks.yml   # external file
```

---

## 📌 Example 2: Split Web Tasks

```yaml
---
- name: web setup
  hosts: web
  become: yes

  tasks:

    - name: import web tasks
      import_tasks: web_tasks.yml
```

---

## 📌 Example 3: Reusable Tasks

```yaml
---
- name: reusable tasks
  hosts: all

  tasks:

    - name: import common setup
      import_tasks: common.yml
```

---

# ⚡ 5. File + Copy + Service (Most Used Industry Pattern)

## 📌 Example 1: Deploy Website

```yaml
---
- hosts: web
  become: yes

  tasks:

    - name: install nginx
      apt:
        name: nginx
        state: present

    - name: copy website files
      copy:
        src: ./website/
        dest: /var/www/html

    - name: start nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

---

## 📌 Example 2: Config file deployment

```yaml
---
- hosts: all
  become: yes

  tasks:

    - name: copy config file
      copy:
        src: ./config.conf
        dest: /etc/app/config.conf
        mode: '0644'
```

---

## 📌 Example 3: Create directory + file

```yaml
---
- hosts: all
  become: yes

  tasks:

    - name: create directory
      file:
        path: /opt/myapp
        state: directory

    - name: create file inside directory
      file:
        path: /opt/myapp/app.log
        state: touch
```

---

# ▶️ Run Ansible Playbook Command

```bash
ansible-playbook playbook.yml
```

Inventory ke sath:

```bash
ansible-playbook -i inventory playbook.yml
```

Check syntax:

```bash
ansible-playbook --syntax-check playbook.yml
```

---

