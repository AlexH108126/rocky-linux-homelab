## Purpose
COnfigure a rocky linux headless server to provide reliable NAS storage over the network using samba, accessible permanently from: Linux clients (mint) and windows 11 clients.
The NAS storage lives on a dedicated partition, mounted persistently, and secured using proper Linux permissions, SELinux labeling, and Samba authentication.


## Installation
- The initial disk state on the system used a GPT disk with:
   - /boot/efi (630MB)
   - /boot (1GB)
   - sda3, the LVM physical volume (1TB)
- inside the 'sda3' LVM:
   - / --*root* (75GB, xfs)
   - /home (920GB, xfs)
   - swap (7GB)
- The plan was to repurpose a portion of the storage for NAS use. I would create a dedicated mount point named, '/srv/nas' and avoid mixing data with '/home'
