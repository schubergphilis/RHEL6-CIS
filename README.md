RedHat Enterprise Linux 6 CIS compliancy.
================
[![Build Status](https://travis-ci.org/schubergphilis/RHEL6-CIS.svg?branch=devel)](https://travis-ci.org/schubergphilis/RHEL6-CIS)

Based on the exellent work done by the MindPointGroup [Ansible Role](https://img.shields.io/ansible/role/16089.svg)](https://galaxy.ansible.com/MindPointGroup/RHEL7-CIS/)

Configure RHEL/Centos 6 machine to be [CIS](https://www.cisecurity.org/cis-benchmarks/) compliant. Level 1 and 2 findings will be corrected by default.

## Warning on use

This role **will make changes to the system** that could break things. This is not an auditing tool but rather a remediation tool to be used after an audit has been conducted.

## IMPORTANT INSTALL STEP

If you want to install this via the `ansible-galaxy` command you'll need to run it like this:

`ansible-galaxy install -p roles -r requirements.yml`

With this in the file requirements.yml:

```
- src: https://github.com/schubergphilis/RHEL6-CIS.git
```

Based on [CIS RedHat Enterprise Linux 6 Benchmark v2.1.0 - 12-27-2017 ](https://community.cisecurity.org/collab/public/index.php).

This repo originated from work done by [Sam Doran](https://github.com/samdoran/ansible-role-stig)

Requirements
------------

You should carefully read through the tasks to make sure these changes will not break your systems before running this playbook.
If you want to do a dry run without changing anything, set the below sections (rhel6cis_section1-6) to false. 

Role Variables
--------------
There are many role variables defined in defaults/main.yml. This list shows the most important.

**rhel6cis_notauto**: Run CIS checks that we typically do NOT want to automate due to the high probability of breaking the system (Default: false)

**rhel6cis_section1**: CIS - General Settings (Section 1) (Default: true)

**rhel6cis_section2**: CIS - Services settings (Section 2) (Default: true)

**rhel6cis_section3**: CIS - Network settings (Section 3) (Default: true)

**rhel6cis_section4**: CIS - Logging and Auditing settings (Section 4) (Default: true)

**rhel6cis_section5**: CIS - Access, Authentication and Authorization settings (Section 5) (Default: true)

**rhel6cis_section6**: CIS - System Maintenance settings (Section 6) (Default: true)  

##### Disable all selinux functions
`rhel6cis_selinux_disable: false`

##### Service variables:
###### These control whether a server should or should not be allowed to continue to run these services

```
rhel6cis_avahi_server: false  
rhel6cis_cups_server: false  
rhel6cis_dhcp_server: false  
rhel6cis_ldap_server: false  
rhel6cis_telnet_server: false  
rhel6cis_nfs_server: false  
rhel6cis_rpc_server: false  
rhel6cis_ntalk_server: false  
rhel6cis_rsyncd_server: false  
rhel6cis_tftp_server: false  
rhel6cis_rsh_server: false  
rhel6cis_nis_server: false  
rhel6cis_snmp_server: false  
rhel6cis_squid_server: false  
rhel6cis_smb_server: false  
rhel6cis_dovecot_server: false  
rhel6cis_httpd_server: false  
rhel6cis_vsftpd_server: false  
rhel6cis_named_server: false  
rhel6cis_bind: false  
rhel6cis_vsftpd: false  
rhel6cis_httpd: false  
rhel6cis_dovecot: false  
rhel6cis_samba: false  
rhel6cis_squid: false  
rhel6cis_net_snmp: false  
```  

##### Designate server as a Mail server
`rhel6cis_is_mail_server: false`


##### System network parameters (host only OR host and router)
`rhel6cis_is_router: false`  


##### IPv6 required
`rhel6cis_ipv6_required: true`  


##### AIDE
`rhel6cis_config_aide: true`

###### AIDE cron settings
```
rhel6cis_aide_cron:
  cron_user: root
  cron_file: /etc/crontab
  aide_job: '/usr/sbin/aide --check'
  aide_minute: 0
  aide_hour: 5
  aide_day: '*'
  aide_month: '*'
  aide_weekday: '*'  
```

##### SELinux policy
`rhel6cis_selinux_pol: targeted` 


##### Set to 'true' if X Windows is needed in your environment
`rhel6cis_xwindows_required: no` 


##### Client application requirements
```
rhel6cis_openldap_clients_required: false 
rhel6cis_telnet_required: false 
rhel6cis_talk_required: false  
rhel6cis_rsh_required: false 
rhel6cis_ypbind_required: false 
```

##### Time Synchronization
```
rhel6cis_time_synchronization: chrony
rhel6cis_time_Synchronization: ntp

rhel6cis_time_synchronization_servers:
    - 0.pool.ntp.org
    - 1.pool.ntp.org
    - 2.pool.ntp.org
    - 3.pool.ntp.org  
```  
  
##### 3.4.2 | PATCH | Ensure /etc/hosts.allow is configured
```
rhel6cis_host_allow:
  - "10.0.0.0/255.0.0.0"  
  - "172.16.0.0/255.240.0.0"  
  - "192.168.0.0/255.255.0.0"    
```  

```
rhel6cis_firewall: firewalld
rhel6cis_firewall: iptables
``` 
  

Dependencies
------------

Ansible > 2.2

Example Playbook
-------------------------

This sample playbook should be run in a folder that is above the main RHEL6-CIS / RHEL6-CIS-devel folder.

```
- name: Harden Server
  hosts: servers
  become: yes

  roles:
    - RHEL6-CIS
```

Tags
----
Many tags are available for precise control of what is and is not changed.

Some examples of using tags:

```
    # Audit and patch the site
    ansible-playbook site.yml --tags="patch"
```

License
-------

MIT
