Virsh Install
=========

Installs VMs on a Hypervisor via the virt-install utility.

Requirements
------------

Ansible >= 2.0

Role Variables
--------------

```role_variables/myvm.yml```
```
vm:
##HV VARS##
    hypervisor: [YOUR HYPERVISOR INVENTORY HOSTNAME]
    location: [YOUR ISO PATH] #Path to image on HV. For now this has to be preexsistent.
    distribution: [Ubuntu] # Distribution name of the VM currently only Ubuntu supported. Debian may work this way aswell since both use preseed.
    mem: "1024" # VM memory in Mbyte
    desc: "[YOUR VM DESRIPTION]" # The VM description string
    net:
      bridge: [YOUR NETWORK BRIDGE] # Currently only bridge is supported
    disk:
      size: "50" # HDD size in GB for the virtual disk
      pool: "[VIRSH POOLNAME]" # libvirt/virsh pool name
##NETWORK VARS##
#    name: [YOUR VM NAME | default(inventory_hostname)] # Your great vm name here
#    hostname: [YOUR HOSTNAME | default(inventory_hostname)] # VM hostname
    ipaddress: "XXX.XXX.XXX.XXX" # IPv4 address
    netmask: "255.255.255.0" # Netmask defaults to a /24 if not set
    gateway: "XXX.XXX.XXX.XXX" # IPv4 Gateway
    nameservers: "XXX.XXX.XXX.XXX" # IPv4 Nameserver
    domain: "my.lan" # Domain of this vm
##PRESEED VARS##
    preseed_ip: "10.29.0.1" # Ip of the hypervisor. Currently needet
    preseed_path: "/" # Gets some meaning in a later stage dont change
    preseed_port: '8000' # Currently not possible to change but needet
##MISC VARS##
    autostart: "yes" # Most people would want their VM to startup on reboot
##FILES AND SSHKEYS##
    files: #Currently this will only manage hardcoded files in the postInstall.sh but should change to a more usefull setup at a later time.
     - content: "[YOUR LONGASS SSH KEY]"
       dest: "authorized_keys"
##OPTIONAL PARAMETERS##
#    keymap: "de" # Defaults to us if not set
#    wipe_existing: True # If you uncomment this exsisting partitions/lvm on the VM storage will be overwritten
#    pass_gpg_id: "0x12345" # Currently unused
#    pass_server_ip: "10.29.0.164" # Currently unused
#    pass_server_port: '8000' # Currently unused
```

How To Use
----------

Create a host_vars file for your vm like above and fill missing info

Example Playbook
----------------

```
    - hosts: myserver
      gather_facts: no #needet due to te vm being non exsistent at first
      roles:
        - maesto.virsh_install #virsh_install _has to_ be the first role. Else the VM won't exsist for the other roles. virt_install by default gathers facts for the VM after installing it.
```
License
-------

BSD

Author Information
------------------

Lucas Wendel <lucas.wendel@igeh.me>
