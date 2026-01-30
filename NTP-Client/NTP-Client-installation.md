## [Purpose]
Classic minimal-install trick. The time daemon isnt automatically installed. Rocky minimal doesnt not ship with an NTP client by default. Cockpit has an option for automatic NTP timeclock but
it will be greyed out if there isnt one installed. You may have time drifts in your logs causing errors, and authentication/security problems come up sporadically.


## [Installation]
- I installed chrony for this rocky server. It allows me to set times auto-synced to a time server.
    - sudo dnf install -y chrony
- this gives me:
    - chronyd (the daemon)
    - chronyc (control tool)
    - and Cockpit NTP integration
- Enabled and started the chrony services
- Enabled NTP at the system level and verified it was configured properly.
    - sudo timedatectl set-ntp true
    - timedatectl: 'system clock syn: yes.' 'NTP services: active'
