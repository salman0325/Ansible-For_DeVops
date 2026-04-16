# 🔥 1. What is Ansible?

👉 **Answer (Interview Style):**
“Ansible is an agentless automation tool used for configuration management, application deployment, and orchestration. It uses SSH to connect to servers and execute tasks.”

👉 **Scenario:**
“In my project, I used Ansible to install and configure web servers on multiple machines in a single run.”

---

# 🔥 2. Why did you choose Ansible?

👉 **Answer:**
“I chose Ansible because it is agentless, easy to learn, and uses simple YAML syntax. It reduces manual effort and speeds up deployments.”

👉 **Scenario:**
“In one project, we were not allowed to install agents on servers, so Ansible was the best choice.”

---

# 🔥 3. What is an Inventory?

👉 **Answer:**
“Inventory is a file that contains the list of managed hosts and groups.”

👉 **Scenario:**
“I created separate inventories for development and production environments.”

---

# 🔥 4. What is a Playbook?

👉 **Answer:**
“A playbook is a YAML file that defines tasks to be executed on managed nodes.”

👉 **Scenario:**
“I created a playbook to automate application deployment across servers.”

---

# 🔥 5. What are Ansible Modules?

👉 **Answer:**
“Modules are reusable units of code used to perform specific tasks like installing packages, copying files, or managing services.”

👉 **Scenario:**
“I used the apt module to install packages on Ubuntu servers.”

---

# 🔥 6. Difference between shell and command?

👉 **Answer:**
“The command module is more secure and does not support shell features, while the shell module supports pipes, redirection, and complex commands.”

👉 **Scenario:**
“I used the shell module when I needed to run commands with pipes.”

---

# 🔥 7. What is `when` in Ansible?

👉 **Answer:**
“‘when’ is used to define conditions in Ansible, similar to an if statement.”

👉 **Scenario:**
“I used ‘when’ to install packages only on Ubuntu systems.”

---

# 🔥 8. What is a loop in Ansible?

👉 **Answer:**
“A loop is used to iterate over a list of items and execute a task multiple times.”

👉 **Scenario:**
“I used a loop to install multiple packages in one task.”

---

# 🔥 9. What is `item`?

👉 **Answer:**
“‘item’ represents the current value in a loop.”

👉 **Scenario:**
“In a loop of packages, ‘item’ refers to each package name.”

---

# 🔥 10. What is `register`?

👉 **Answer:**
“Register is used to store the output of a task so it can be used later.”

👉 **Scenario:**
“I stored command output and used it in conditions for the next task.”

---

# 🔥 11. What is Idempotency?

👉 **Answer:**
“Idempotency means running the same playbook multiple times will produce the same result without making unnecessary changes.”

👉 **Scenario:**
“If a package is already installed, Ansible will not install it again.”

---

# 🔥 12. What is a Handler?

👉 **Answer:**
“A handler is a task that runs only when notified by another task, usually used for restarting services.”

👉 **Scenario:**
“I used a handler to restart nginx only when its configuration changed.”

---

# 🔥 13. What are Roles?

👉 **Answer:**
“Roles are a way to organize playbooks into reusable components.”

👉 **Scenario:**
“I created a role for web server setup that included tasks, variables, and handlers.”

---

# 🔥 14. group_vars vs host_vars?

👉 **Answer:**
“group_vars are variables applied to a group of hosts, while host_vars are specific to individual hosts.”

👉 **Scenario:**
“I used group_vars for environment-level settings and host_vars for server-specific configs.”

---

# 🔥 15. Variable precedence?

👉 **Answer:**
“Variable precedence defines which variable overrides another. Extra vars have the highest priority.”

👉 **Scenario:**
“I used extra vars to override default values at runtime.”

---

# 🔥 16. What are Facts?

👉 **Answer:**
“Facts are system information collected automatically by Ansible, like OS, IP, and memory.”

👉 **Scenario:**
“I used facts to run different tasks based on OS type.”

---

# 🔥 17. How do you handle failures?

👉 **Answer:**
“I use ignore_errors or define custom failure conditions using failed_when.”

👉 **Scenario:**
“For non-critical tasks, I allowed the playbook to continue even if they failed.”

---

# 🔥 18. What is `until`?

👉 **Answer:**
“‘until’ is used to retry a task until a condition becomes true.”

👉 **Scenario:**
“I used it to wait for a service to start before continuing.”

---

# 🔥 19. What is Ansible Vault?

👉 **Answer:**
“Ansible Vault is used to encrypt sensitive data like passwords.”

👉 **Scenario:**
“I stored database credentials securely using Vault.”

---

# 🔥 20. What are Tags?

👉 **Answer:**
“Tags allow us to run specific tasks from a playbook.”

👉 **Scenario:**
“I used tags to run only deployment tasks without executing the full playbook.”

---

# 🔥 21. Scenario Question (VERY IMPORTANT ⭐)

👉 **Q: How would you deploy nginx on 100 servers?**

👉 **Answer:**
“I would create an inventory file with all servers, write a playbook using the apt module to install nginx, and use the service module to start and enable it. Then I would run the playbook using ansible-playbook.”

---

# 🔥 22. Scenario: Multi-OS Deployment

👉 **Answer:**
“I would use ‘when’ conditions based on ansible_os_family to handle different package managers like apt and yum.”

---

# 🔥 23. Scenario: Application Deployment

👉 **Answer:**
“I automated deployment by pulling code from Git, installing dependencies, and restarting services using handlers.”

---

# 🔥 24. Scenario: Zero Downtime

👉 **Answer:**
“I used rolling updates with the ‘serial’ keyword to update servers in batches.”

---

# 🔥 25. Scenario: Debugging

👉 **Answer:**
“I use verbose mode (-vvv), debug module, and check logs to troubleshoot issues.”

---

# 🔥 FINAL INTERVIEW TIP 💯

👉 Always speak like this:

✔ “In my project, I used…”
✔ “I implemented…”
✔ “I automated…”

👉 Avoid:
❌ “I think…”
❌ “Maybe…”

---

