# Ansible Role: vlag_1tier_spine - Single Layer vLAG Spine Configuration
---
<add role description below>

This is a template driven playbook for a single layer vLAG configuration. The role applies to the vLAG peers. These switches will be referred to as spine switches.

The templates used in this configuration example are the same. They will be used to configure the spine switches. The templates will be executed with different settings for the peer IP configuration that are suitable to each spine switch. The peer IP has to be specified for each spine switch in the */etc/anisble/hosts* file with the tag ID `peerip`. Since there are two spine switches, there will be two sets of values used in the templates.

The configuration commands and their results can be verified in the *commands* and *results* directories.

For more details, see [Configuring a single layer vLAG network](http://systemx.lenovofiles.com/help/index.jsp?topic=%2Fcom.lenovo.switchmgt.ansible.doc%2Fconfiguring_a_single_layer_vlag_using_ansible.html&cp=0_3_1_0_6).


## Requirements
---
<add role requirements information below>

- Ansible version 2.2 or later ([Ansible installation documentation](http://docs.ansible.com/ansible/intro_installation.html))
- Lenovo switches running CNOS version 10.2.1.0 or later
- an SSH connection to the Lenovo switch (SSH must be enabled on the network device)


## Role Variables
---
<add role variables information below>

The values of the variables used need to be modified to fit the specific scenario in which you are deploying the solution. To change the values of the variables, you need to visits the *vars* directory of each role and edit the *main.yml* file located there. The values stored in this file will be used by Ansible when the template is executed.

The syntax of *main.yml* file for variables is the following:

```
<template variable>:<value>
```

You will need to replace the `<value>` field with the value that suits your topology. The `<template variable>` fields are taken from the template and it is recommended that you leave them unchanged.

Available variables are listed below, along with description:

Variable | Description
--- | ---
`username` | Specifies the username used to log into the switch
`password` | Specifies the password used to log into the switch
`flag` | Configures the condition flag associated with the switch
`slot_chassis_number1` | Specifies the ethernet port (*slot number/port number*)
`portchannel_interface_number1` | Specifies the LAG number (*1-4096*)
`portchannel_mode1` | Configures the LAG type (**on** - static LAG, **active** - active member of a LACP LAG, **passive** - passive member of a LACP LAG)
`slot_chassis_number2` | Specifies the ethernet port (*slot number/port number*)
`portchannel_interface_number2` | Specifies the LAG number (*1-4096*)
`slot_chassis_number3` | Specifies the ethernet port (*slot number/port number*)
`portchannel_interface_number3` | Specifies the LAG number (*1-4096*)
`switchport_mode1` | Configures the switch port mode (**access** - the port can be part of only a single VLAN, **trunk** - the port can be part of any number of VLANs)
`stp_mode1` | Configures the STP mode (**mst** - MSTP, **rapid-pvst** - Rapid PVST+, **disable** - STP is disabled)
`vlag_tier_id1` | Configure the vLAG tier ID (*1-512*)
`vlag_instance_number1` | Configure the vLAG instance (*1-64*)
`vlag_instance_number2` | Configure the vLAG instance (*1-64*)


## Dependencies
---
<add dependencies information below>

- username.iptables - Configures the firewall and blocks all ports except those needed for web server and SSH access.
- username.common - Performs common server configuration.
- /etc/ansible/hosts - You must edit the */etc/ansible/hosts* file with the device information of the switches designated as spine switches. You may refer to *vlag_1tier_spine_hosts* for a sample configuration.

Ansible keeps track of all network elements that it manages through a hosts file. Before the execution of a playbook, the hosts file must be set up.

Open the */etc/ansible/hosts* file with root privileges. Most of the file is commented out by using **#**. You can also comment out the entries you will be adding by using **#**. You need to copy the content of the hosts file for the role into the */etc/ansible/hosts* file. The hosts file for the role is located in the main directory of the multiple layer vLAG configuration solution.

```
[vlag_1tier_spine]
10.240.178.76   username=<username> password=<password> deviceType=g8272_cnos peerip=10.240.178.77
10.240.178.77   username=<username> password=<password> deviceType=g8272_cnos peerip=10.240.178.76
```
**Note:** You need to change the IP addresses, including the IP addresses of the vLAG peers, to fit your specific topology. You also need to change the `<username>` and `<password>` to the appropriate values used to log into the specific Lenovo network devices.

  
## Example Playbook
---
<add playbook samples below>

To execute an Ansible playbook, use the following command:

```
ansible-playbook vlag_1tier_spine.yml -vvv
```

`-vvv` is an optional verbos command that helps identify what is happening during playbook execution. The playbook for each role of the single layer vLAG configuration solution is located in the main directory of the solution.

```
- hosts: vlag_1tier_spine
  roles:
    - vlag_1tier_spine
```



## License
---
<add license information below>
Copyright (C) 2017 Lenovo, Inc.

This Ansible Role is distributed WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  

See the [GNU General Public License](http://www.gnu.org/licenses/) for more details.