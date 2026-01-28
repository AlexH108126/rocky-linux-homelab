## Rocky Linux 9.7 Installation - Headless Server Setup:
- hardware: HP Beats Laptop
- Role: Headless homelab server (Pi-hole, Cockpit, NAS)


## Hardware
Device
- laptop model: HP Beats 15 Notebook
- CPU: AMD A10-7300 Radeon R6, 10 Compute Cores 4C+6G
- RAM: 8GiB SODIMM DDR3 1600 MHZ
- Storage: 1TB internal HDD
- Network: PCIe Ethernet controller 100Mbit/s capacity


## Installation Media
1. Download of Rocky Linux 9.7 ISO, Minimal Install variant; No GUI, less resource utilization, services added manually later.
2. Create bootable USB. Used Rufus as installer tool.
3. Boot laptop from USB. Set USB media as first boot option in UEFI.


## User & Security Setup
- Root account: enabled
- Primary user created: 'Beats'
- wheel group access: enabled (given sudo privileges)
- Password authentication: enabled initially (SSH hardened later)

## !Issues experienced after first boot!
- dnf updates failed. COuld not download latest repo. Noticed server had no network configurations/no DHCP offer.
- NIC was present but disconnected, meaning no internet access.
- Network interfaces are not automatically broguht up. Discovered this by using 'NetworkManager.'
    - nmcli device status
Solutions:
- brought NIC interface up and i was able to update server with 'dnf update -y.'
    - nmcli con up ens1


## Headless Server Setup
- Goal: even if lid closes, the laptop will stay on and no go to sleep/suspend.
- Edited the systemd logind config file and restarted logind.
- Made sure SSH was installed, enabled, and worked properly. Hardened it by disabling root logins and requiring password authentication. Then, restarted sshd.
- Removed DC battery from laptop. AC powered via the charging adapter.
