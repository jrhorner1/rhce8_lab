# Red Hat Certified Engineer 8 (EX294)
## Lab Environment

This repository contains the setup files for my lab environment to study for the RHCE 8 exam (EX294). The environment has been setup to the specifications found in the [Red Hat RHCE 8 (EX294) Cert Guide][1] by Sander van Vugt.

It is recommended to use Red Hat Enterprise Linux for the lab environment to match the exam environment. A free developer subscription can be obtained at https://developer.redhat.com.

### Quick Start

Clone this repo, install the software listed under Host Requirements below, and run `vagrant up`. Once the machines are up you can ssh to node0: `vagrant ssh node0` and switch to the ansible user: `sudo su - ansible`. The synced folder is located at `/synced`.  

### Host Requirements

Without getting too into the weeds about hardware, your computer should be capable of running several virtual machines without suffering a major performance hit.  

In addition, the following software will need to be installed:

- Virtualbox  
- Vagrant  
- Ansible 
- nfs-utils

Please note that ansible can only be installed on linux. Window Subsystem for Linux (WSL) can be used but it is [not supported][2]. Additionally the NFS synced folder [will not work in Windows][5]. You will need to modify the vagrant file to use some other method if you want to sync files back to your host.

### Node Requirements

The labs require three (3) virtual machines; one control node and 2 managed nodes. 

|Item                |Requirement            |
|--------------------|-----------------------|
|Operating System    | RHEL 8.x or CentOS 8.x|
|RAM                 |1 GB or more           |
|Disk space          |20 GB or more          |
|Installation profile|Minimal installation   |

|Hostname            |IP           |Description            |
|--------------------|-------------|-----------------------|
|control.example.com |192.168.4.200|Ansible control machine|
|ansible1.example.com|192.168.4.201|First managed node     |
|ansible2.example.com|192.168.4.202|Second managed node    |

Each node is to have an `ansible` user.

### Vagrant Provisioning setup

By default, the Vagrantfile is setup to use the `generic/rhel8` [box][3]. If you wish to use RHEL, you will also need the developer subscription and to have setup an activation key. Populate the variables in `group_vars/all.yaml` with your activation key name and your organization ID number. For added security, use ansible-vault to secure these variables.

You may also use the `generic/centos8` [box][4], or any other CentOS 8 or RHEL 8 build of your choice. No additional setup should be required for CentOS boxes. 

While counter productive to the point of this repo, you may also choose an OS outside of the Red Hat family to use, in which case a pip task is available to install ansible on the control node. 

[1]: https://www.amazon.com/RHCE-EX294-Cert-Guide-Certification/dp/0136872433/ref=tmm_pap_swatch_0?_encoding=UTF8&qid=&sr=
[2]: https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html#can-ansible-run-on-windows
[3]: https://app.vagrantup.com/generic/boxes/rhel8
[4]: https://app.vagrantup.com/generic/boxes/centos8
[5]: https://www.vagrantup.com/docs/synced-folders/nfs