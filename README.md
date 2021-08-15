# Internet Pi

**A Project for Internet connectivity monitoring, network-wide ad-blocking and local DNS, intended for running on a Raspberry Pi**

[![Ansible Lint](https://github.com/TimGrt/internet-pi/actions/workflows/ci.yml/badge.svg)](https://github.com/TimGrt/internet-pi/actions/workflows/ci.yml) [![CodeFactor](https://www.codefactor.io/repository/github/timgrt/internet-pi/badge)](https://www.codefactor.io/repository/github/timgrt/internet-pi)
## Features

**Internet Monitoring**: Installs Prometheus and Grafana, along with a few Docker containers to monitor the Internet connection with Speedtest.net speedtests and HTTP tests so uptime, ping stats, and speedtest results can be seen over time.

![Internet Monitoring Dashboard in Grafana](/images/internet-monitoring.png)

**Pi-hole**: Installs and configures a Pi-hole Docker instance for network-wide ad-blocking and local DNS. Update the network router config to direct all DNS queries through the Raspberry Pi!

![Pi-hole on the Internet Pi](/images/pi-hole.png)

## Recommended Pi and OS

Use a Raspberry Pi 4 model B or better. The Pi 4 and later generations of Pi include a full gigabit network interface and enough I/O to reliably measure fast Internet connections.

Older Pis work, but have many limitations, like a slower CPU and sometimes very-slow NICs that limit the speed test capability to 100 Mbps or 300 Mbps on the Pi 3 model B+.

Other computers and VMs may run this configuration as well, but it is only regularly tested on a Raspberry Pi.

The configuration is tested against Raspberry Pi OS, both 64-bit and 32-bit, and runs great on that or a generic Debian installation.

It should also work with Ubuntu for Pi, or Arch Linux, but has not been tested on other operating systems.

## Setup

1. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html). The easiest way is via Pip:
```bash
pip3 install ansible
```
2. Clone this repository, then enter the repository directory: `cd internet-pi`.
```bash
git clone https://github.com/TimGrt/internet-pi.git
```
3. Install requirements:
```bash
ansible-galaxy collection install -r requirements.yml
```
4. Adjust the `inventory.ini` with the hostname/IP address of your Pi.
5. Run the playbook:
```bash
ansible-playbook main.yml
```

> **If running locally on the Pi**: You may encounter an error like "Error while fetching server API version". If you do, please either reboot or log out and log back in, then run the playbook again.

## Usage

### Pi-hole

Visit the Pi's IP address (e.g. http://192.168.1.10/) and use the `pihole_password` which is set in the defaults folder of the role.

> Note: The `pihole_password` should be changed to a more secure password! The password is the actual one for the PiHole UI!

Either change the variable in the defaults folder or set it in e.g. *host_vars* and encrpyt it, if you intend to store the project in a publicly accessible place.  
When running the playbook again, provide the Vault password.
```bash
$ ansible-vault encrypt host_vars/internetpi.yml
New Vault password: 
Confirm New Vault password: 
Encryption successful
$ ansible-playbook main.yml --ask-vault-pass
```

### Grafana

Visit the Pi's IP address with port 3030 (e.g. http://192.168.1.10:3030/), and log in with username `admin` and the password `monitoring_grafana_admin_password` which is set in the defaults folder of the role.

> Note: The `monitoring_grafana_admin_password` is only used the first time Grafana starts up, use the updated password.


## Author

This project was created in 2021 by [Jeff Geerling](https://www.jeffgeerling.com/) and adjusted for my personal needs.  
