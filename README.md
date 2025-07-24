# Linux Server Setup & Hardening

This project demonstrates how to set up and harden a Linux (Ubuntu) server for secure and reliable operation. The server is prepared with security best practices including SSH hardening, firewall configuration, intrusion prevention, and automatic updates.

---

## Project Overview

- Created a non-root user for administrative tasks
- Hardened SSH access by disabling root login and password authentication
- Configured UFW firewall to allow only essential ports
- Installed and configured Fail2ban to protect against brute-force attacks
- Enabled automatic security updates to keep the system patched

---

## Steps Performed

### 1. System Update

sudo apt update && sudo apt upgrade -y

### 2. Create a New User and Grant Sudo Privileges

sudo adduser yourusername

sudo usermod -aG sudo yourusername

### 3. Harden SSH Configuration

- Disabled root login and password authentication in /etc/ssh/sshd_config:

PermitRootLogin no

PasswordAuthentication no

- Restarted SSH service:

sudo systemctl restart sshd

### 4. Configure UFW Firewall

sudo ufw allow OpenSSH

sudo ufw enable

sudo ufw status

### 5. Install and Configure Fail2ban

sudo apt install fail2ban -y

sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local


- Enabled SSH jail in /etc/fail2ban/jail.local:

[sshd]
enabled = true
port    = ssh
logpath = %(sshd_log)s
maxretry = 5

- Restarted Fail2ban:

sudo systemctl restart fail2ban

- Verified Fail2ban status:

sudo fail2ban-client status sshd

### 6. Enable Automatic Security Updates

sudo apt install unattended-upgrades -y

sudo dpkg-reconfigure --priority=low unattended-upgrades

---

## Testing & Verification

- Attempted SSH login as root (expected: failure)
- Tested password authentication (expected: failure)
- Confirmed firewall allows only SSH port (22)
- Triggered Fail2ban by multiple failed login attempts to ensure IP ban

---

## Why This Matters

Implementing these steps significantly reduces the attack surface of the server by:

- Preventing unauthorized root access
- Enforcing key-based SSH authentication
- Limiting open network ports to only what is necessary
- Detecting and blocking repeated failed login attempts
- Ensuring the system receives security patches automatically

---

## References

- Ubuntu Server Guide - Security: https://ubuntu.com/server/docs/security
- Fail2ban Documentation: https://www.fail2ban.org/wiki/index.php/Main_Page
- UFW Firewall Documentation: https://help.ubuntu.com/community/UFW
- SSH Hardening Tips: https://www.ssh.com/academy/ssh/hardening

---

