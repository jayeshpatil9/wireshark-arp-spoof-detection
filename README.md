# ARP Spoofing Attack Detection with Wireshark

## Objective
This project sets out to detect and analyze Layer 2 Address Resolution Protocol (ARP) cache poisoning (MITRE ATT&CK T1557.002). It matters because ARP is a stateless protocol lacking native authentication, meaning hosts inherently trust unsolicited replies. This vulnerability allows an adversary to forge MAC-to-IP bindings, successfully establishing a Man-in-the-Middle (MitM) position to intercept, modify, or block local network traffic.

## Environment
The simulation was conducted inside an isolated VirtualBox internal network (`intnet` - `192.168.10.0/24`) to contain broadcast traffic safely:

| Role | OS | IP Address | MAC Address |
| :--- | :--- | :--- | :--- |
| **Attacker / Analyst** | Kali Linux (`eth0`) | `192.168.10.10` | `08:00:27:63:b0:05` |
| **Victim Host** | Ubuntu VM (`enp0s3`) | `192.168.10.20` | `08:00:27:38:ee:b2` |
| **Spoofed Target** | Simulated Gateway | `192.168.10.1` | *Impersonated by Attacker* |

![Network Topology](screenshots/01-network-topology.png)

## MITRE ATT&CK Mapping
* **T1557.002:** Adversary-in-the-Middle: ARP Cache Poisoning
* **Tactics:** Credential Access, Collection
* **Data Sources:** Network Traffic (Packet Capture), Host ARP Cache Inspection

## Attack Execution
To emulate the adversary, network interfaces were verified and baseline communications established. Static configurations confirmed endpoint identities before initiating the attack workflow.

First, Kali Linux and Ubuntu verified interface parameters and baseline ARP connectivity:
* **Kali IP Configuration:**
  ```bash
  ip a show eth0