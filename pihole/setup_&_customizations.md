## Installation
- Installed prerequisites, 'dnf install -y curl'
- Installed Pi-hole, 'curl -sSL https://install.pi-hole.net | bash'
- During setup: chose LAN interface, public upstream DNS (Quad9), accept defaults
- COnfigured pihole with a static IP using DHCP reservation and set it as default DNS. Also, Set pi-hole as DNS for server itself using NetworkManagement (nmcli).
- verified i was able to access web GUI and entered in default credentials, which i later changed and added 2FA to login.


## !Problems when first connecting to web GUI!
1. received error messages when first attempting to access pi-hole web GUI.
2. network traffic was not being filtered through pi-hole even after giving it static IP and setting it up as default DNS on home router.
Solution:
1. Checked the firewall status on server, 'firewall-cmd --list-services,' which did no show http as allowed. I then set an allow/outgoing http port 80 on firewall.
Firewalld is set to deny/incoming, allow/outgoing traffic. I then set SELinux to permissive instead of enforcing, making it less restrictive when explicit rules are added to firewalld table.
2. Made sure all devices connected to home network were using the pi-hole DNS filtering by Setting an allow/ougoing rule for DNS port 53 on the server firewall.
