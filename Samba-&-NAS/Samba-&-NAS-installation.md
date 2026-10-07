## [Purpose]
Setup rocky linux headless server to use as local NAS storage using samba software. The NAS storage lives on a dedicated partition, mounted persistently, and secured using proper Linux permissions, SELinux, and Samba authentication.


## [Samba & NAS setup/installation]
- The initial disk state on the system used a GPT disk with:
    - sda3, the LVM physical volume (1TB)
- inside the 'sda3' LVM:
    - / --*root* (75GB, xfs)
    - /home (920GB, xfs)
    - swap (7GB)
- I repurpose a portion of the storage for NAS use. I created a dedicated directory, '/srvr/nas' to avoid mixing data with '/home' folder.
- I began by creating the NAS directory, set ownership, and establish permissions.
    - sudo mkdir -p /srv/nas
    - sudo chown -R nassrv:nassrv /srvr/nas
    - sudo chmod 2775 /srvr/nas
- I installed samba, enabled and started the services, and edited the samba configuration file.
- I added a samba user with its own credentials and enabled the new samba user.
    - sudo smbpasswd -a nassrv
    - sudo smbpasswd -e nassrv
- In the '/etc/samba/smb.conf' file, i added specific configurations for my samba setup. Then, restarted samba services.

## [Splitting '/home' storage:]
- For the NAS storage, i split the '/home' directory into 2 separate partitions: '/home' will be 120GB, '/srvr/nas' will be 600GB, and i extended the '/ ' root directory with the last 200GB.
- the steps i took:
    - killed all processes and unmounted '/home'
    - deleted the Logical Volume, instantly making the 920GB into free space
    - created the 600GB NAS logical volume, formatted 'dev/rl/nas' xfs, mounted to NAS directory, and made it persistent by adding it to '/etc/fstab'
    - then, i remade the '/home' with 120GB and extended the root logical volume '/ ' with the 200GB of free space left
 

## [Mounting NAS permanently to remote clients]
- Windows clients (simple):
    - opened file explorer
    - right-clicked 'This PC'
    - then 'Map network drive,' chose a drive letter (e.g. L:)
    - input the NAS storage path '\\SERVER_IP\nas,' and enter samba credentials, check off 'Reconnect at sign-in,' and check off 'Connect using different credentials.'
- Linux clients (trickier):
    - installed 'cifs-utils' package on the client machine
    - create a mount point
    - create a credentials file in '/root' directory for added security. Changed file permissions to '600', meaning only file owner can read & write to it
    - make the mount persistent by editing the '/etc/fstab' file













## [Split Partitions]
![storage_info](https://github.com/user-attachments/assets/28f4d7f9-61cd-4edd-bd17-4cea7562cfb0)
## [NAS Mounted on Remote Client]
![nas_verified](https://github.com/user-attachments/assets/ce316941-b700-4733-a751-f30e12ae8a7c)



