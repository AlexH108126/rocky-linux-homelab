## Purpose
I wanted to see what made DNS function and operate properly. The goal was to have a more in-depth understanding of how a DNS server worked and what services made it efficient. I did not use unbound services as a local DNS resolver this time as that needs more configuring and troubleshooting.


## Installation
- Installed prerequisites, 'dnf install -y curl'
- Installed Pi-hole, 'curl -sSL https://install.pi-hole.net | bash'
- During setup: chose LAN interface, public upstream DNS (Quad9), accept defaults
- COnfigured pihole with a static IP using DHCP reservation on my home router and set it's IP as the default DNS for the entire network. Also, Set the server's default DNS address to be its own localhost address (127.0.0.1) using NetworkManager tool (nmcli).
- verified i was able to access web GUI and entered in the default credentials, which i later changed and added 2FA to the pi-hole GUI login.


## !Troubleshooting pi-hole!
1. received error messages when first attempting to access pi-hole web GUI.
2. network traffic was not being filtered through pi-hole after giving it a static IP and setting it up as default DNS on home router.

Solutions:
1. Checked the firewall status on server, 'firewall-cmd --list-services,' which did not show http on the allowed list. I then set an allow/outgoing http port 80 on firewall.
Firewalld is set to deny/incoming, allow/outgoing traffic. I then set SELinux to permissive instead of enforcing, making it less restrictive when explicit rules are added to firewalld table.
2. Made sure all devices connected to home network were using the pi-hole DNS filtering by Setting an allow/outgoing rule for DNS port 53 on the server firewall.
