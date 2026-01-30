## Purpose
Cockpit will be used as a web-based system management for my headless rocky linux server running on repurposed laptop hardware. The goal is to manage services, storage, network settings, and logs without installing a full desktop environment. This saves on resource utilization and can be remotely managed on any computer/smartphone connected to my network.


## Installation
- Cockpit is already included in the rocky linux repo by default. optionally, you can download it if it's not.
    - sudo dnf install cockpit -y
- It runs as a systemd service and listens on TCP port 9090. you enable/confirm its service using, 'systemctl.'
    - sudo systemctl enable --now cockpit.socket
    - systemctl status cockpit.socket
- once enabled, you can access the cockpit GUI from a browser by inputting, 'http://SERVER_IP:9090.' the default port number for cockpit GUI is 9090, but you can change it later on if you wish.


## !Troubleshooting Cockpit!
1. Cockpit web UI was unreachable from the browser. connection to the http path failed.
2. SELinux warnings related to samba and custom NAS directories where identified thanks to cockpit's GUI visuals.
3. Mislabeled directories were popping up in SELinux logs.
4. Confirmed that functionality was not being interrupted due to these warnings.

Solutions:
1. Root cause of unreachable http path was that firewalld was blocking port 9090 by default. Added a permanent rule on the firewall telling it allow listening on port 9090.
2. Samba and NAS warnings were due to incorrect/missing permissions on the client side of the connections. Added the correct samba permissions to the clients connecting to the remote NAS storage.
3 & 4. Cockpit began reporting SELinux alerts saying, 'preventing /path/to/smbd from search access on dir /path/nas.' Though the NAS was fully functional and clients could access it. Fix was to simply label the shared directories correctly from, 'unlabeled_t' to 'samba_share_t' and apply label persistency. Thankfully, SELinux show hints and command suggestions to fix these errors, making the troubleshooting simple and straightforward for a novice like myself.
