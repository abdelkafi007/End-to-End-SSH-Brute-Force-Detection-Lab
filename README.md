# End-to-End-SSH-Brute-Force-Detection-Lab
📌 Project Overview
This project demonstrates the setup of a complete Cybersecurity Home Lab to simulate, detect, and analyze a brute-force attack on a Linux server. The goal was to build an attack pipeline and a defense pipeline to monitor security events in real-time using a SIEM solution.

🏗️ Architecture
Hypervisor: VirtualBox (Bridged Networking Mode)

Victim Machine: Ubuntu Server 24.04 LTS (Restricted resources: 1GB RAM)

Attacker Machine: Parrot Security OS 6.4 (Live Mode)

SIEM (Defense): Wazuh Cloud

🔧 Tools Used
Hydra: For performing the dictionary-based brute-force attack.

Wazuh Agent: For log collection and endpoint monitoring.

SSH: The target protocol for the attack.

RockYou.txt: The wordlist used for password cracking.

🚀 Implementation Steps
1. Lab Environment Setup
Deployed Ubuntu Server as the target, configured with an intentionally weak user (admin1:123456) and Open SSH server enabled.

Deployed Parrot Security as the attacker.

Configured Bridged Networking to allow peer-to-peer communication between VMs and the internet.

2. The Attack Phase (Red Team)
Used Hydra to launch a brute-force attack against the victim's SSH service (Port 22).

Command Used:
Bash

hydra -l admin1 -P /usr/share/wordlists/rockyou.txt 192.168.8.27 ssh -t 4

Outcome: Successfully cracked the password (123456) and generated thousands of authentication failure logs.
<img width="423" height="54" alt="attack" src="https://github.com/user-attachments/assets/a4e50a9c-04fa-47bd-88a9-5b3b9d37f903" />
<img width="642" height="75" alt="attack_ok" src="https://github.com/user-attachments/assets/1e6bab1a-c2f1-44bc-8506-1583c2bad3ee" />


3. The Defense Phase (Blue Team)
Installed the Wazuh Agent on the Ubuntu Victim.

Connected the agent to the Wazuh Cloud Dashboard.

Configured the agent to monitor /var/log/auth.log for suspicious activity.

4. Detection & Analysis
The Wazuh SIEM successfully ingested the logs.
<img width="297" height="230" alt="brute force" src="https://github.com/user-attachments/assets/924acb2a-fcc6-4c49-9e66-a90bfe2865e8" />

Triggered Alerts:

Rule ID 5716: SSHD authentication failed (Repeated failures).

Rule ID 5712: SSHD brute force trying to get access to the system.

Rule ID 5710: Attempt to login using a non-existent user.

Attribution: Successfully identified the Attacker's IP (192.168.8.27) in the alert details.
<img width="418" height="130" alt="ipadress" src="https://github.com/user-attachments/assets/58d42e60-2481-4880-a67f-bdb124f99a09" />
