# 🔥 1. Loop (basic + hash + nested)

## ✅ Simple loop

```yaml
- name: install multiple packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - nginx
    - git
    - curl
```

---

## ✅ Hash loop (dictionary loop)

```yaml
- name: create users
  user:
    name: "{{ item.name }}"
    uid: "{{ item.uid }}"
  loop:
    - { name: "ali", uid: 1011 }
    - { name: "ahmed", uid: 1012 }
```

👉 Yahan `item.name` aur `item.uid` use ho raha hai

---

## ✅ Nested loop

```yaml
- name: install packages for users
  debug:
    msg: "{{ item.0 }} installs {{ item.1 }}"
  loop:
    - [ "ali", "nginx" ]
    - [ "ahmed", "git" ]
```

---

# 🔥 2. Handlers + Notify

👉 Handler tab run hota hai jab change ho

```yaml
- name: install nginx
  apt:
    name: nginx
    state: present
  notify: restart nginx

handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
```

👉 Agar nginx already installed hai → handler run nahi hoga

---

# 🔥 3. Block, Rescue, Always (try-catch jaisa)

```yaml
- name: block example
  block:
    - name: install package
      apt:
        name: nginx
        state: present

  rescue:
    - name: error msg
      debug:
        msg: "Installation failed"

  always:
    - name: always run
      debug:
        msg: "This always runs"
```

👉

* `block` → try
* `rescue` → catch
* `always` → finally

---

# 🔥 4. Vault (password hide karna)

## Create vault file:

```bash
ansible-vault create secret.yml
```

Example:

```yaml
db_password: mysecret123
```

## Use in playbook:

```yaml
vars_files:
  - secret.yml
```

## Run:

```bash
ansible-playbook site.yml --ask-vault-pass
```

---

# 🔥 5. Roles (MOST IMPORTANT 🔥)

## 📁 Role structure

```bash
ansible-galaxy init webrole
```

Structure:

```
webrole/
  tasks/main.yml
  handlers/main.yml
  vars/main.yml
  templates/
  files/
```

---

## ✅ Role example (NGINX static website)

### 👉 tasks/main.yml

```yaml
- name: install nginx
  apt:
    name: nginx
    state: present
    update_cache: yes

- name: copy website
  copy:
    src: index.html
    dest: /var/www/html/index.html
  notify: restart nginx
```

---

### 👉 handlers/main.yml

```yaml
- name: restart nginx
  service:
    name: nginx
    state: restarted
```

---

### 👉 files/index.html

```html
<h1>Welcome Salman DevOps 🚀</h1>
```

---

## ✅ Main playbook

```yaml
- hosts: all
  become: yes
  roles:
    - webrole
```

---

# 🔥 6. Static Website Deploy (Full Flow)

```bash
ansible-playbook -i inventory site.yml
```

👉 Browser me:

```
http://your-server-ip
```

---

# 🔥 REAL DEVOPS FLOW (IMPORTANT)

Tum ye sab combine kar sakte ho:

```yaml
- name: deploy website
  hosts: all
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

    - name: copy site
      copy:
        src: index.html
        dest: /var/www/html/index.html
      notify: restart nginx

  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```

---

