## [Purpose]
COnfigure a rocky linux headless server to provide reliable NAS storage over the network using samba, accessible permanently from: Linux clients (mint) and windows 11 clients.
The NAS storage lives on a dedicated partition, mounted persistently, and secured using proper Linux permissions, SELinux labeling, and Samba authentication.


## [Samba & NAS setup/installation]
- The initial disk state on the system used a GPT disk with:
    - /boot/efi (630MB)
    - /boot (1GB)
    - sda3, the LVM physical volume (1TB)
- inside the 'sda3' LVM:
    - / --*root* (75GB, xfs)
    - /home (920GB, xfs)
    - swap (7GB)
- The plan was to repurpose a portion of the storage for NAS use. I would create a dedicated mount point named, '/srvr/nas' and avoid mixing data with '/home.'
- I began by creating the NAS directory, set ownership, and establish permissions ('nassrv' means specifically for NAS access, 'setgid' (2) makes the exec run with privileges of the group of the file)
    - sudo mkdir -p /srv/nas
    - sudo chown -R nassrv:nassrv /srvr/nas
    - sudo chmod 2775 /srvr/nas
- I installed samba from the rocky repos, enabled and started the services, and edited the samba configuration file with custom procedures.
- I added a samba user with its own credentials and enabled the new samba user.
    - sudo smbpasswd -a nassrv
    - sudo smbpasswd -e nassrv
- In the '/etc/samba/smb.conf' file, i inserted custom content specific to my samba setup. Then, restarted samba services on server.

## [Splitting '/home' storage:]
- For the NAS storage, i split the '/home' directory into 2 separate partitions: '/home' will be 120GB, '/srvr/nas' will be 600GB, and i added the last 200GB into the '/ ' root directory.
- the steps i took:
    - killed processes and unmounted '/home'
    - removed the Logical Volume, instantly making the 920GB into free space
    - created the 600GB NAS logical volume, formatted 'dev/rl/nas' xfs, mounted to NAS directory, and made it persistent by adding to '/etc/fstab'
    - then, i recreated the '/home' as a 120GB directory and extended the root logical volume '/ ' with the 200GB left of free space
 

## [Mounting NAS permanently to remote clients]
- Windows clients (simple):
    - opened file explorer
    - right-clicked 'This PC'
    - then 'Map network drive,' chose a drive letter (e.g. L:)
    - input the NAS storage path '\\SERVER_IP\nas,' and enter samba credentials, check off 'Reconnect at sign-in,' and check off 'Connect using different credentials.'
- Linux clients (trickier):
    - installed 'cifs-utils' package on the client machine
    - create a mount point
    - create a credentials file in '/root' directory for added security, contents should be 'username=nassrv <TAB> password=sambapassword.' secure it by changing permissions of file to '600'
    - make the mount persistent by editing the '/etc/fstab' file
