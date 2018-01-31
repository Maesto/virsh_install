Virsh Install
=========

Installs VMs on a Hypervisor via the virt-install utility.

Requirements
------------

Ansible >= 2.0

Role Variables
--------------
```
virsh_install_guests:
  - name: "My VM Name" # Your great vm name here
    keymap: "de" # Defaults to us if not set
#    wipe_existing: True # If you uncomment this exsisting partitions/lvm on the VM storage will be overwritten
    kernelparams: "url=http://10.29.0.1:8000/preseed.cfg" #Currently needet will move to a var include
    ipaddress: "XXX.XXX.XXX.XXX" # IPv4 address
    netmask: "255.255.255.0" # Netmask defaults to a /24 if not set
    gateway: "XXX.XXX.XXX.XXX" # IPv4 Gateway
    nameservers: "XXX.XXX.XXX.XXX" # IPv4 Nameserver
    domain: "my.lan" # Domain of this vm
    hostname: "My hostname" # VM hostname
    pass_gpg_id: "0x12345" # Currently unused
    pass_server_ip: "10.29.0.164" # Currently unused
    pass_server_port: '8000' # Currently unused
    preseed_ip: "10.29.0.1" # Ip of the hypervisor. Currently needet.
    preseed_path: "/" # Gets some meaning in a later stage dont change.
    preseed_port: '8000' # Currently not possible to change but needet. 
    autostart: "yes" # Most people would want their VM to startup on reboot
    mem: "1024" # VM memory in Mbyte
    desc: "TesterVM" # The VM description string
    net:
      bridge: "intern" # Currently only bridge is supported
    disk:
      size: "50" # HDD size in GB for the virtual disk
      pool: "vg_md0" # libvirt/virsh pool name
    location: "/var/lib/libvirt/images/ubuntu-16.04.3-server-amd64.iso" #Path to image on HV currently has to be exsistent.
    templates: # Needed don't change for now will probably move to a vars import
      - src: "preseed.cfg.j2"
        dest: "preseed.cfg"
      - src: "postInstall.sh.j2"
        dest: "postInstall.sh"
   files: # Needed don't change for now will probably move to a vars import
      - src: "authorized_keys"
        dest: "authorized_keys"
```

How To Use
----------

Remember to add your authorized_keys file to the file folder or any other location where a ansible file copy will find it.


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: maesto.virsh_install }

License
-------

BSD

Author Information
------------------

Lucas Wendel <lucas.wendel@igeh.me>
