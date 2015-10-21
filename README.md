Role Name
=========

Common base role for all hosts. Should be ran after mrlesmithjr.bootstrap role.
(Configure apt-cacher,update dhcp client,update dns servers,configure lvm)

[![Build Status](https://travis-ci.org/mrlesmithjr/ansible-base.svg?branch=master)](https://travis-ci.org/mrlesmithjr/ansible-base)
Requirements
------------

If creating, resizing or creating LVM volumes/volume groups ensure that you define the correct current disk and the new disk(s) added. If you choose to use apt-cacher ensure that you already have a server built and configured correctly for apt-caching.

Role Variables
--------------

````
---
# defaults file for ansible-base
apt_cacher_server: []  #define apt-cacher server if used...define here or globally in group_vars/all/servers
config_lvm: false  #defines if lvm should be configured when adding additional disks...should be defined in host_vars/host
config_monit: false  #defines if monit is to be configured if installed
create_local_users: true  #defines creating local user accounts on hosts
create_lvm: false  #defines if lvm should be created when adding additional disks...should be defined in host_vars/host (do not set extend or resize to true)
create_lvname: test-lv  #define lvm name when adding additional disks...should be defined in host_vars/host
create_lvsize: 100%FREE  #defines the lvm lv size --- (10G) - would create new lvm with 10Gigabytes -- (512) - would create new lvm with 512m
# To generate passwords use (replace P@55w0rd with new password).... echo "P@55w0rd" | mkpasswd -s -m sha-512
create_users:  #defines user accounts to setup on hosts....define here or in group_vars/all
  - user: demo_user  #define username
    authorized_keys: ''
    comment: 'Demo user'  #define a comment to associate with the account
    generate_keys: false  #generate ssh keys...true|false
    home: ''  #define a different home directory... ''=/home/username
    pass: demo_password  #define password for account
    setup: false  #true=creates account|false=removes account if exists...true|false
    shell: ''  #define a different shell for the user
    sudo: false  #define if user should have sudo access...true|false
    system_account: false  #define if account is a system account...true|falseinstall_fail2ban: false
create_vgname: test-vg  #defines the lvm vg name to create...
current_disk: /dev/sda5  #defines the disk currently configured for lvm...should be defined in host_vars/host
extend: false  #defines if lvm vg should be extended (do not set create to true)...should be defined in host_vars/host
=======
deploy_ssh_pub_keys:  #defines remote users to add ssh pub keys for either the remote user or adding another users pub key to a remote user for passwordless ssh
  - remote_user: demo_user
    keys:
      - ssh_pub_keys/demo_user.pub
#      - ssh_pub_keys/demo_user_1.pub
#  - remote_user: demo_user2
#    keys:
#      - ssh_pub_keys/demo_user2.pub
enable_deploy_ssh_pub_keys: false  #defines if accounts in deploy_ssh_pub_keys should be managed
extend_lvm: false  #defines if lvm vg should be extended (do not set create to true)...should be defined in host_vars/host
extend_disks: '{{ current_disk }},{{ new_disk }}'  #defines the disks to extend in lvm group...should be defined in host_vars/host
extend_lvname: test-lv  #defines the lvm lv name to extend...should be defined in host_vars/host
extend_vgname: test-vg  #defines the lvm vg name to extend...should be defined in host_vars/host
install_monit: false  #defines if mrlesmithjr.monit role should be installed.
lvm_filesystem: ext4  #defines the filesystem type to create when configuring lvm ( ext3, ext4, xfs, etc. )...should be defined in host_vars/host
lvextend_options: '-L+10G'  #defines the options to pass to lvextend --- ('-L+10G') - would increase by 10GB whereas ('-l +100%FREE') would increase to full capacity
lvm_new_disk: /dev/sdb  #defines the new disk being added to volume group...should be defined in host_vars/host
lvm_new_mntp: /mnt/test  #defines the desired mount point to be created and new logical volume to be mounted to...should be defined in host_vars/host
resize_lvm: false  #set to true if resizing the logical volume (do not set create to true)...should be defined in host_vars/host
resize_lvname: test-lv  #defines the logical volume name to resize...should be defined in host_vars/host
resize_vgname: test-vg  #defines the volume group name to resize...should be defined in host_vars/host
rundeck_generate_resources_xml: false  #defines if rundeck CI resources.xml should be generated if using rundeck...
rundeck_server: []  #define server that is running rundeck CI if used...define here or globally in group_vars/all/servers
ssh_manage_ssh_known_hosts: false  #define if hosts ssh_known_hosts should be managed
update_dhcpclient_conf: false  #defines if dhcp client config should be updated...define here or globally in group_vars/all/configs
update_dns_nameservers: false  #defines if dns servers should be updated...define here or globally in group_vars/all/configs
update_dns_search: false  #defines if dns search domain should be updated...define here or globally in group_vars/all/configs
update_etc_hosts: false  #defines if /etc/hosts should be updated...define here or globally in group_vars/all/configs
````

Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: mrlesmithjr.base }

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com
