# Day 8: Install Ansible

**Difficulty**: 🟢 Beginner  
**Category**: Configuration Management / DevOps  
**Estimated Time**: 10 minutes  

---

## 🎯 Objective

Install **Ansible version 4.7.0** on the **Jump Host** using `pip3` and ensure that the **Ansible command is available globally**, so all users on the system can run Ansible commands.

---

## 📖 Scenario

During the weekly meeting, the **Nautilus DevOps team** discussed different automation and configuration management tools.  
After evaluating multiple options, the team decided to use **Ansible** due to its simple setup and minimal prerequisites.

To start testing automation tasks, the team selected the **Jump Host** as the **Ansible Controller** to manage and execute tasks on other servers.

---

## 📋 Prerequisites

- Access to the Jump Host  
- `sudo` privileges  
- Python 3 installed  
- `pip3` available  
- Internet access  

---

## 🛠 Tools & Technologies

- Linux (Jump Host)  
- Python 3  
- pip3  
- Ansible (v4.7.0)  

---

## 🔧 Implementation Steps

### Step 1: Verify Python and pip3

```sh
python3 --version
pip3 --version
```

If `pip3` is not installed:

```sh
sudo apt install python3-pip -y
```

### Step 2: Install Ansible Using pip3

Install Ansible version 4.7.0 globally:

```sh
sudo pip3 install ansible==4.7.0
```

Using `sudo` ensures Ansible is installed system-wide and accessible to all users.

### Step 3: Verify Ansible Installation

```sh
ansible --version
```

Expected output contains `ansible` version 4.7.0 and Python details.

✅ Verification Checklist

- Ansible installed successfully
- Correct Ansible version is available
- `ansible` command works for all users
- No installation errors

🔧 Troubleshooting

Issue: `ansible: command not found`

- Ensure installation was done with `sudo`
- Check Ansible binary location:

```sh
which ansible
```

Issue: Wrong Ansible version installed

```sh
sudo pip3 uninstall ansible
sudo pip3 install ansible==4.7.0
```

🧠 Key Learnings

- Ansible is an agentless automation tool
- Installing via `pip3` allows version control
- Global installation ensures system-wide access
- Jump Host acts as the Ansible Controller

🔐 Best Practices

- Always pin Ansible to a specific version
- Use a dedicated controller host
- Verify installation before automation tasks
- Maintain documentation for setup steps
