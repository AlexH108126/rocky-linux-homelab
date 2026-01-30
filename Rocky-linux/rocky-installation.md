## [Rocky Linux 9.7 Installation - Headless Server Setup:]
- hardware: HP Beats Laptop
- Role: Headless homelab server (Pi-hole, Cockpit, NAS)


## [Hardware]
Device
- laptop model: HP Beats 15 Notebook
- CPU: AMD A10-7300 Radeon R6, 10 Compute Cores 4C+6G
- RAM: 8GiB SODIMM DDR3 1600 MHZ
- Storage: 1TB internal HDD
- Network: PCIe Ethernet controller 100Mbit/s capacity


## [Installation Media]
1. Download of Rocky Linux 9.7 ISO, Minimal Install variant; No GUI, less resource utilization, services added manually later.
2. Create bootable USB. Used Rufus as installer tool.
3. Boot laptop from USB. Set USB media as first boot option in UEFI.


## [User & Security Setup]
- Root account: enabled
- Primary user created: 'Beats'
- wheel group access: enabled (given sudo privileges)
- Password authentication: enabled initially (SSH hardened later)

## [!Issues experienced after first boot!]
- dnf updates failed. Could not download latest repo. Noticed server had no network configurations/no DHCP offer.
- NIC interface was present but was disconnected, meaning that laptop could not connect to DHCP server and obtain the configurations necessary for internet access.
- Network interfaces are not automatically up in some minimal installs. Discovered this by using 'NetworkManager.'
    - nmcli device status
Solutions:
- brought the NIC interface up and it was able to communicate with the DHCP serve and get the proper network configurations.
    - nmcli con up ens1
- Finally, I updated the server with the latest packages using,'dnf update -y.'


## [Headless Server Setup]
- Goal: even if lid closes, the laptop will stay on and not go to sleep/suspend. Allowing for remote access via cockpit GUI or SSHing into the CLI.
- Edited the logind.conf file to disable the lid switch power0ff triggers, and then restarted logind to run with the new settings.
- Made sure SSH tools were installed, enabled, and worked properly. Hardened SSH by disabling root logins and requiring password authentication. Then, restarted sshd.
- Removed the physical DC battery from the laptop. It will now be AC powered via the charging adapter being plugged permanently. The DC battery now acts as emergency power backup.
