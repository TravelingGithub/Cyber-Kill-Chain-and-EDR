# EDR-and-Cyber-Kill-Chain

- [EDR-and-Cyber-Kill-Chain](#edr-and-cyber-kill-chain)
  - [Project:](#project)
  - [Setup](#setup)
    - [Adversary - Ubuntu Server](#adversary---ubuntu-server)
    - [Victim - Windows 11 Machine](#victim---windows-11-machine)
    - [Windows Defender Antivirus Disabled](#windows-defender-antivirus-disabled)
  - [Cyber Kill Chain Framework](#cyber-kill-chain-framework)
    - [Reconnaissance Phase](#reconnaissance-phase)
    - [Weaponization Phase](#weaponization-phase)
    - [Delivery Phase](#delivery-phase)
    - [Exploitation Phase](#exploitation-phase)
    - [Installation Phase](#installation-phase)
    - [Command and Control Phase](#command-and-control-phase)
    - [Actions on Objective Phase](#actions-on-objective-phase)
  - [Lessons Learned](#lessons-learned)


## Project: 
The project features one adversary and one victim, utilizing Endpoint Detection and Response (EDR) technology and the Cyber Kill Chain framework for simulation presentation.

## Setup
The adversary machine is an Ubuntu server running Sliver. After setting up the Ubuntu Server, an SSH connection was established from the host machine to the server for the rest of the exercise. The victim endpoint is a Windows 11 machine with Windows Defender Antivirus disabled and a cloud EDR solution, the LimaCharlie agent, installed. System Monitor (Sysmon) will be creating the logs. During setup, an attempt was made to permanently disable Windows Defender antivirus, but this failed. The workaround for this exercise was to restart the attack each time Windows Defender Antivirus was enabled again. The permanent fix would be to use a custom image of a Windows machine with Windows Defender Antivirus completely removed.

### Adversary - Ubuntu Server
![1 Setup - Attack IP ](/Images/1%20Setup%20-%20Attack%20IP%20.PNG)

### Victim - Windows 11 Machine
![2 Setup - Defend IP ](/Images/2%20Setup%20-%20Defend%20IP.PNG)

### Windows Defender Antivirus Disabled
![3 Setup - Defense Disabled](/Images/3%20Setup%20-%20Defense%20Disabled.PNG)

## Cyber Kill Chain Framework
### Reconnaissance Phase
First would be the reconnaissance phase, since the target machine is already known, this step can be bypassed for the exercise. In the real world, however, thorough reconnaissance is crucial, as it helps identify potential vulnerabilities and understand the network environment, even when the target is known. Gathering information about the system’s configuration, services running, and any exposed interfaces can provide valuable insights that inform subsequent phases of the operation, ensuring a more effective and stealthy approach. I did include the screenshots of the gathering information steps as a substitute for the recon phase since this is after the Command and Control (C2) phase.
![1 Kill Chain Reconnaissance](/Images/1%20Kill%20Chain%20Reconnaissance.PNG)


### Weaponization Phase
Using Sliver, a C2 payload was generated. The victim Windows 11 machine would later identify the malware as a Trojan.
![4 Kill Chain Weaponization](/Images/4%20Kill%20Chain%20Weaponization.PNG)

### Delivery Phase
To simulate a delivery phase, a Python server was run on the Ubuntu server using port 80, and the victim machine downloaded the malware from the Python server.
![5 Kill Chain Delivery](/Images/5%20Kill%20Chain%20Delivery%20-%202.PNG)

### Exploitation Phase
Simulated a click and use of the malware.
![6 Kill Chain Exploitation](/Images/6%20Kill%20Chain%20Exploitation.PNG)

### Installation Phase
A backdoor was installed, but the first attempt failed as Windows detected a Trojan. The main issue was that Windows Security's real-time protection could not be disabled. I was able to disable Windows Security by enabling Group Policy Object Editor to "Turn Off Windows Defender Antivirus" and then using "gpupdate /force." However, this is a temporary fix, as after a restart or Windows updates, the setting will likely revert. For future projects, a custom image without Windows Security may be used.
![7 Kill Chain Install- Threats Found](/Images/7%20Kill%20Chain%20Install-%20Threats%20Found.PNG) 

### Command and Control Phase
Persistent access to the victim machine.
![8 Kill Chain C2](/Images/8%20Kill%20Chain%20C2%20-2.PNG)

### Actions on Objective Phase
The command "procdump -n lsass.exe -s lsass.dmp" was used on the victim machine. This command saves a snapshot as a file named lsass.dmp, which contains important details about the process, such as active threads and loaded modules, as well as sensitive information like security tokens and password hashes. This information is very useful for an attacker. The real-time feed shows the log but no detection rules on the EDR.
![9 Kill Chain Actions on Objectives](/Images/9%20Kill%20Chain%20Actions%20on%20Objectives.PNG)

A rule was created to alert when any process attempts to access lsass.exe. However, this rule is likely to produce a high volume of false positives due to frequent legitimate access by authorized applications.
![EDR Rule](/Images/10%20EDR%20Rule.PNG)

An alert was reported.
![Detection and Analysis NIST Incident Response Life Cycle 2](/Images/Detection%20and%20Analysis%20NIST%20Incident%20Response%20Life%20Cycle%202.PNG)

## Lessons Learned
An Endpoint Detection and Response (EDR) system is important for any organization’s cybersecurity plan. It acts as a line of defense against advanced threats targeting endpoints in a network. EDR helps security teams monitor and respond to potential attacks in real time, making it easier to spot and fix problems quickly.
