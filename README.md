
# TechCrush Cybersecurity Capstone Project
## Lab 1: Virtual Machine Networking and SSH Configuration

**Programme:** TechCrush Academy — Cybersecurity  
**Project:** Group Capstone Project  
**Lab Component:** Kali Linux and Ubuntu Virtual Networking  
**Status:** Completed and Verified  
**Purpose:** Group Presentation and Capstone Integration

---

## 1. Project Overview

This lab forms part of our larger TechCrush Academy Cybersecurity Capstone Project.

The objective was to configure two virtual machines on the same network, verify connectivity using Ping, and establish secure remote access using SSH.

This component will be presented to my group members for review before integration into our main capstone project and final defence.

### Objectives
- Install Kali Linux and Ubuntu Desktop in VirtualBox.
- Configure both machines on the same virtual network.
- Verify IP addresses and network connectivity.
- Install and enable SSH on Ubuntu.
- Access Ubuntu remotely from Kali Linux.
- Document results with screenshots.

---

## 2. Lab Environment

| Component | Configuration |
|---|---|
| Host OS | Windows |
| Hypervisor | Oracle VirtualBox |
| Security Workstation | Kali Linux |
| Target Machine | Ubuntu Desktop 26.04.1 LTS |
| Network Mode | NAT Network |
| Network Name | CyberLab |
| Network Subnet | 192.168.77.0/24 |
| Kali IP | 192.168.77.3 |
| Ubuntu IP | 192.168.77.4 |
| SSH Port | TCP 22 |

### Network Topology

```text
        Windows Host Computer
            Oracle VirtualBox
                   |
          NAT Network: CyberLab
             192.168.77.0/24
                   |
          -------------------
          |                 |
      Kali Linux       Ubuntu Desktop
    192.168.77.3       192.168.77.4
    SSH Client         SSH Server
          |                 |
          ------- SSH -------
```

---

## 3. Implementation Steps

### Step 1: Install Virtual Machines

- Installed Kali Linux as the security workstation.
- Installed Ubuntu Desktop as the target machine.
- Configured both operating systems in Oracle VirtualBox.

Verified Ubuntu installation:

```bash
lsb_release -a
```

Checked network interfaces:

```bash
ip -4 addr
```

Verified internet connectivity:

```bash
ping -c 4 ubuntu.com
```

**Result:** Ubuntu installation and internet connectivity successful.

![Ubuntu Installation](screenshots/01-ubuntu-installation.png)

### Step 2: Identify Initial Network Issue

Initially, both virtual machines displayed:

```text
Kali Linux: 10.0.2.15
Ubuntu:     10.0.2.15
```

This occurred because both machines were using VirtualBox's default NAT mode with separate virtual NAT environments.

**Solution:** Configure a shared NAT Network.

### Step 3: Configure Shared NAT Network

In VirtualBox:

1. Shut down both virtual machines.
2. Open Tools > Network > NAT Networks.
3. Create a network named `CyberLab`.
4. Set IPv4 prefix to `192.168.77.0/24`.
5. Enable DHCP.
6. Open each VM's Settings > Network > Adapter 1.
7. Select NAT Network and choose `CyberLab`.
8. Restart both machines.

Verify IP addresses on both machines:

```bash
ip -4 addr
```

**Results:**

```text
Kali Linux: 192.168.77.3/24
Ubuntu:     192.168.77.4/24
```

Both machines now have unique IP addresses within the same subnet.

![Kali IP Configuration](screenshots/02-kali-ip.png)

![Ubuntu IP Configuration](screenshots/03-ubuntu-ip.png)

### Step 4: Test Network Connectivity

From the Kali Linux terminal:

```bash
ping -c 4 192.168.77.4
```

**Result:**

```text
4 packets transmitted
4 packets received
0% packet loss
```

This confirmed successful IP connectivity between Kali Linux and Ubuntu.

![Successful Ping](screenshots/04-ping-success.png)

### Step 5: Install and Enable SSH on Ubuntu

Update package information:

```bash
sudo apt update
```

Install OpenSSH Server:

```bash
sudo apt install openssh-server -y
```

Enable and start SSH:

```bash
sudo systemctl enable --now ssh
```

Verify SSH service status:

```bash
sudo systemctl status ssh
```

**Result:**

```text
Active: active (running)
SSH Port: 22
```

If UFW firewall is enabled, allow SSH:

```bash
sudo ufw allow OpenSSH
```

![SSH Service Running](screenshots/05-ssh-running.png)

### Step 6: Connect Kali Linux to Ubuntu Using SSH

From Kali Linux:

```bash
ssh capstone@192.168.77.4
```

After authenticating with the Ubuntu password, verify remote access:

```bash
whoami
hostname
hostname -I
```

**Results:**

```text
capstone
capstone-VirtualBox
192.168.77.4
```

This confirmed successful remote access from Kali Linux to Ubuntu using SSH.

To terminate the SSH session:

```bash
exit
```

![Successful SSH Connection](screenshots/06-ssh-success.png)

---

## 4. Test Results

| Test | Result | Status |
|---|---|---|
| Ubuntu Installation | Successful | PASS |
| Shared Network Configuration | Successful | PASS |
| IP Address Verification | Unique IPs | PASS |
| Ping Connectivity | 0% Packet Loss | PASS |
| SSH Server | Active and Running | PASS |
| Remote SSH Login | Successful | PASS |

---

## 5. Challenges and Solutions

**Challenge:** Both machines initially had the IP address `10.0.2.15`.

**Cause:** Separate VirtualBox default NAT environments.

**Solution:** Created a shared NAT Network named `CyberLab`, enabling communication between both virtual machines.

**Lesson Learned:** Devices require appropriate network configuration to communicate, and SSH provides encrypted remote administration.

---

## 6. Contribution to the TechCrush Capstone

This lab establishes a working virtual networking and secure remote-access environment.

The completed configuration will be demonstrated to fellow group members, reviewed, and considered for integration into the larger TechCrush Cybersecurity Capstone Project.

### Next Steps

- Present practical results to group members.
- Gather feedback and recommendations.
- Integrate the validated configuration into the main project.
- Prepare for the final capstone defence.

---

## 7. Conclusion

Successfully configured Kali Linux and Ubuntu on the same VirtualBox NAT Network, verified connectivity using Ping, and established secure remote access using SSH.

The exercise strengthened my practical understanding of Linux administration, virtual networking, IP addressing, connectivity troubleshooting and secure remote access.

**TechCrush Academy | Cybersecurity Capstone Project**
