## [Purpose]
To secure remote access to a Rocky Linux server by enforcing SSH key-based authentication only, disabling password authentication, changing the default ssh port, configuring a legal/security login banner, 
and supporting Linux/Windows clients. The Rocky server acts as the centrol SSH target and specific clients initiate connections using private keys only.


## [Installation]
- I used OpenSSH as the daemon on both client machines.
- A key must be generated so that the server can authenticate the clients.
Linux mint:
    - ssh-keygen -t ed25519 -C "mint-client"
    - default location: ~/.ssh/id_ed25519, enter a passphrase when prompted for one
    - Install the public keys on the rocky server: ssh-copy-id -i ~/.ssh/id_ed25519.pub user@SERVER_IP
    - keys are appended to: ~/.ssh/authorized_keys
Windows 11:
    - open up PuTTYgen
    - select 'Type: ed25519'
    - click generate
    - save as: private key (.ppk), public key (copy text). **Private key neve leaves the clients
    - SSH into server and edit authorized keys: sudo nano ~/.ssh/authorized_keys
    - paste in the public key and set permissions: chmod 600 ~/.ssh/authorized_keys
- Verify key-based login works before any added hardening or you will be locked out remotely.
    - ssh user@SERVER_IP
 

## [Hardening SSH]
- begin by editing the SSH daemon configuration file. It is recommended to make a backup copy of this file in case you get locked out and have to reset to default settings.
    - sudo vi /etc/ssh/sshd_config
    - change port number from 22 to a custom one **above 1024
    - uncommnet public key authentication and set to 'yes'
    - uncomment password & challenge response authentication and set to 'no'
    - uncomment permnit root login and set to 'no'
    - uncomment banner and insert ,/etc/issue.net' if using a banner
 

## [Firewalld configuration]
- if you've changed the default port number for SSH, you will need to add that new port to the firewall allow list.
    - sudo firewall-cmd --permanent --add-port=3456/tcp. Then, restart your server for settings to work.
- optionally, you can disable port 22 if you've changed the port number to something else.
 

## [Banner (optional)]
- start by creating a banner file
    - sudo nano /etc/issue.net
    - insert text you want shown every time someone start an SSH session with your server. EX: ======WARNING! AUTHORIZED USE ONLY.======
 
