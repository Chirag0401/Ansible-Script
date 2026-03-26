```markdown
# Service Status Checker using Ansible

## 📌 Overview
This Ansible playbook connects to multiple servers using provided `adm_user` credentials and checks the status of specified systemd services.

It prompts for:
- SSH username (`adm_user`)
- SSH password (also used for sudo)

The playbook then executes `systemctl is-active` for each service and displays the status.

---

## ⚙️ Prerequisites

- Ansible installed on control node
- Network connectivity to target servers
- SSH access using `adm_user`
- `adm_user` must have **sudo privileges**
- Python installed on target servers

---

## 📂 Project Structure

```

.
├── inventory.ini
├── check_service_status.yml
└── README.md

```

---

## 📝 Inventory Setup

Create an `inventory.ini` file:

```ini
[servers]
server1 ansible_host=10.10.10.11
server2 ansible_host=10.10.10.12
server3 ansible_host=10.10.10.13
````

---

## 🧾 Playbook Configuration

Edit the services inside the playbook:

```yaml
services_to_check:
  - besclient.service
  - falcon-sensor.service
  - ir_agent.service
```

---

## ▶️ Execution Command

Run the playbook using:

```bash
ansible-playbook -i inventory.ini check_service_status.yml
```

---

## 🔐 Prompts During Execution

You will be prompted for:

```text
Enter adm user:
Enter adm user password:
```

> Note: The same password is used for SSH and sudo.

---

## 📊 Sample Output (Pseudo)

```text
PLAY [Check status of selected services on target servers] *****************

TASK [Check service status using systemctl] ********************************
ok: [server1] => (item=besclient.service)
ok: [server1] => (item=falcon-sensor.service)
ok: [server1] => (item=ir_agent.service)

ok: [server2] => (item=besclient.service)
ok: [server2] => (item=falcon-sensor.service)
ok: [server2] => (item=ir_agent.service)

TASK [Print service status] ************************************************
ok: [server1] => (item=besclient.service) =>
  msg: "Host=server1 | Service=besclient.service | Status=active"

ok: [server1] => (item=falcon-sensor.service) =>
  msg: "Host=server1 | Service=falcon-sensor.service | Status=active"

ok: [server1] => (item=ir_agent.service) =>
  msg: "Host=server1 | Service=ir_agent.service | Status=inactive"

ok: [server2] => (item=besclient.service) =>
  msg: "Host=server2 | Service=besclient.service | Status=active"

ok: [server2] => (item=falcon-sensor.service) =>
  msg: "Host=server2 | Service=falcon-sensor.service | Status=failed"

ok: [server2] => (item=ir_agent.service) =>
  msg: "Host=server2 | Service=ir_agent.service | Status=not found"

PLAY RECAP ***************************************************************
server1 : ok=2  changed=0  unreachable=0  failed=0
server2 : ok=2  changed=0  unreachable=0  failed=0
```

---

## 💡 Notes

* Uses `systemctl is-active` for quick and clean status checks
* `ignore_errors: yes` ensures playbook continues even if a service is missing
* `--no-pager` is avoided for cleaner output
* Works best on **systemd-based Linux systems (RHEL, CentOS, Ubuntu, etc.)**


## 👨‍💻 Author

Chirag Sharma
DevOps Engineer

```
