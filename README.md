# 🛡️ Ubuntu 22.04 Secure Server Hardening with Ansible

This project provides a **fully automated Ubuntu 22.04 server-hardening setup** using **Ansible**, with **optional Vagrant support** for users who do not have an existing server.

The goal is to help security engineers, DevOps teams, and cloud practitioners deploy a hardened baseline quickly and cleanly, following security best practices.

---

## 🚀 Overview

This setup performs automated hardening on **any Ubuntu 22.04 server** by applying:

- System updates & patches  
- Strong password policies  
- SSH hardening (disable root login, disable password login, apply clean config)  
- UFW firewall rules  
- Login banners  
- Kernel-level sysctl security rules  

It is designed so that:

- You **run Ansible from a Linux machine** (Kali Linux, Ubuntu, WSL2 Ubuntu, etc.)  
- Your target server may be:
  - A local VM  
  - A cloud VM  
  - A physical machine  
- **Vagrant is optional** - used only if you do not have a server to test on.

---

## 📦 Technologies Used

- **Ubuntu 22.04 LTS**
- **Ansible (latest version)**
- **Vagrant (optional)**
- **VirtualBox** (for optional VM testing)
- **SSH + YAML-based automation**

---

## 📁 Folder Structure

```
ubuntu-ansible-secure-server/
│
├── README.md
│
├── ansible/
│   ├── inventory.ini
│   ├── playbook.yml
│   │
│   └── roles/
│       └── hardening/
│           ├── tasks/
│           │   ├── banners.yml
│           │   ├── main.yml
│           │   ├── ssh.yml
│           │   ├── sysctl.yml
│           │   ├── ufw.yml
│           │   ├── updates.yml
│           │   └── users.yml
│           │
│           └── templates/
│               ├── issue.net.j2
│               ├── sshd_config.j2
│               └── sysctl.conf.j2
│
└── vagrant/
    └── Vagrantfile
```

---

## 🧰 Installing Ansible (if not installed)

On any Linux machine (Kali / Ubuntu / WSL2):

```bash
sudo apt update
sudo apt install ansible -y
```

Check version:

```bash
ansible --version
```

---

## 📥 Clone the Repository

Before running anything, clone the project:

```bash
git clone https://github.com/Mr3bd/ubuntu-ansible-secure-server.git
cd ubuntu-ansible-secure-server/ansible
```

---

## 🧩 About Vagrant (Optional)

Vagrant is included **only as a testing option** if you do NOT have an Ubuntu server.

Why optional?

- Many users already have cloud VMs or on-prem servers.
- Ansible works with SSH to any machine.

If you choose to use Vagrant:

1. Install Vagrant & VirtualBox  
2. Start the VM:

```bash
cd vagrant
vagrant up
```

3. Your server will be accessible at:

```
192.168.56.10
```

4. Use the **Vagrant insecure private key** (already included in your system at this path):

```
~/.vagrant.d/insecure_private_key
```

---

## 🔑 First SSH Connection Must Be Manual

When connecting to any new server, SSH creates a fingerprint entry in:

```
~/.ssh/known_hosts
```

If this file does not contain the target fingerprint yet, Ansible **will refuse to connect**.

So you must SSH manually once:

```bash
ssh vagrant@192.168.56.10 -i ~/.vagrant.d/insecure_private_key
```

Accept the fingerprint, then exit:

```bash
exit
```

Now Ansible can connect normally.

---

## ⚙️ Configure inventory.ini

Edit:

```
ansible/inventory.ini
```

Set:

- Target server IP  
- SSH username  
- SSH private key path  

Example for Vagrant:

```
[ubuntu_servers]
vagrant ansible_host=192.168.56.10 ansible_user=vagrant ansible_private_key_file=~/.vagrant.d/insecure_private_key
```

Example for your own server:

```
[ubuntu_servers]
prod ansible_host=YOUR_SERVER_IP ansible_user=ubuntu ansible_private_key_file=~/.ssh/id_rsa
```

*Note: "prod" is simply a label used as the host name inside Ansible. It does not affect anything on the actual server.*

---

## ▶️ Run the Hardening Playbook

From inside the `ansible/` directory:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

Expected output:

- Several “changed” tasks (first run)
- No failures
- Server hardened automatically

---

## 🔍 How to Verify Hardening

### 1. Check UFW rules
```bash
sudo ufw status
```

### 2. Check SSH config
```bash
sudo cat /etc/ssh/sshd_config
```

### 3. Check sysctl security parameters
```bash
sudo sysctl -a | grep kernel
sudo sysctl -a | grep net.ipv4
```

### 4. Check login banner
```bash
cat /etc/issue.net
```

---

## 🔐 What This Project Hardens

### ✔ SSH Hardening
- Disable root login  
- Disable password authentication  
- Only key authentication allowed  
- Clean, optimized SSH configuration  

### ✔ Password Policy Enforcement
- Minimum length 12  
- Requires uppercase, lowercase, digits, and symbols  

### ✔ Kernel Security Rules (sysctl)
- Disable IP source routing  
- Disable packet redirects  
- Enable SYN cookies  
- Improve TCP security  

### ✔ UFW Firewall
- Allow SSH  
- Deny all others  
- Enable firewall on boot  

### ✔ Legal Banners
- `/etc/issue.net`  
- Enabled in SSH  

---

## 📌 Notes

- Ansible **must run from Linux**  
- Target server can be anywhere (cloud / local / VM)  
- The machine running Ansible must reach the server over SSH  
- Vagrant is **optional**  
- Works on **Ubuntu 22.04 only**  

---

## 🌟 SEO Metadata

**Keywords:**  
Ubuntu hardening, Ansible hardening, Linux security automation, DevSecOps, SSH hardening, UFW firewall rules, sysctl security, Vagrant Ubuntu 22.04, cloud security baseline

**Description:**  
A complete Ubuntu 22.04 server hardening automation using Ansible, with optional Vagrant support. Includes SSH security, firewall configuration, password policies, kernel hardening, banners, and update management.

---

## 👨‍💻 Author

**Abdullrahman Wasfi**  
LinkedIn: https://www.linkedin.com/in/mr3bd  
Website: https://mr3bd.com  

Made with passion for **Cloud Security, DevOps, and Automation**.
