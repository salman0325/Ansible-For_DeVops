## 🔹 1. Basic Ansible Commands

* Hosts check karne ke liye:

```bash
ansible all -m ping
```

* Inventory file specify karne ke liye:

```bash
ansible all -i hosts -m ping
```

* Kisi specific group par run:

```bash
ansible webservers -m ping
```

---

## 🔹 2. Ad-hoc Commands (Direct commands run karna)

* Command run karna:

```bash
ansible all -m command -a "uptime"
```

* Shell command run:

```bash
ansible all -m shell -a "df -h"
```

* Root (sudo) ke sath:

```bash
ansible all -m apt -a "name=nginx state=present" --become
```

---

## 🔹 3. File & Copy Commands

* File copy:

```bash
ansible all -m copy -a "src=file.txt dest=/tmp/file.txt"
```

* File download (URL se):

```bash
ansible all -m get_url -a "url=http://example.com/file dest=/tmp/file"
```

---

## 🔹 4. Package Management

* Ubuntu/Debian:

```bash
ansible all -m apt -a "name=nginx state=present"
```

* CentOS/RHEL:

```bash
ansible all -m yum -a "name=httpd state=present"
```

---

## 🔹 5. Service Management

```bash
ansible all -m service -a "name=nginx state=started"
```

Restart:

```bash
ansible all -m service -a "name=nginx state=restarted"
```

---

## 🔹 6. User Management

```bash
ansible all -m user -a "name=ali state=present"
```

Delete user:

```bash
ansible all -m user -a "name=ali state=absent"
```

---

## 🔹 7. Playbook Commands

* Playbook run:

```bash
ansible-playbook playbook.yml
```

* Inventory ke sath:

```bash
ansible-playbook -i hosts playbook.yml
```

* Dry run (check mode):

```bash
ansible-playbook playbook.yml --check
```

---

## 🔹 8. Facts Check (System info)

```bash
ansible all -m setup
```

---

## 🔹 9. Debug / Test

```bash
ansible all -m debug -a "msg='Hello World'"
```

---

## 🔹 10. Useful Options

* Verbose mode:

```bash
ansible all -m ping -v
```

* Extra verbose:

```bash
ansible all -m ping -vvv
```

---

