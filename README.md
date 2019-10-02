RGM Ansible plyabook
====================

This is the Ansible playbook for **Rigby Group Monitoring** aka. [RGM](https://rgm-could.io) installation.

Without any customization, the playbook will install and configure **RGM** on the Ansible host (in other terms, on
*localhost* itself). If this is not what you expect, then please set the **inventory** file to feet your needs !

Note also that althrough this *playbook* can work by itself, it is usually lanched through a *bash shell* script also
maintained by the **RGM community** and available here: [rgm-installer.sh](https://installer.rgm-cloud.io/rgm-installer.sh)

The main goal of this script is:
  * ensure target compatibility (hardware & software compliance)
  * adds EPEL repository
  * adds RGM official repository
  * adds RGM repo GPG key
  * install some prerequisites (git, ansible)

The script supports an inline help, a typical usage could be:

    bash <(curl -k -s https://installer.rgm-cloud.io/rgm-installer.sh) -y


Playbook's roles dependencies
-----------------------------

The playbook depends on 3 Ansible roles, all maintained by the **RGM community**:

| role name | description |
|-----------|-------------|
| [role-lvm-partitionner](https://framagit.org/rgm-community/ansible/roles/role-lvm-partitionner) | This role creates LVM PVs, VGs, LVs, formats the LV, add the corresponding mount-points, and ensure the new volume are correctly mounted |
| [role-snmp](https://framagit.org/rgm-community/ansible/roles/role-snmp)                         | This role installs a SNMP agent on target server, configures a default SNMP v1/v2 community for RGM (in RO) |
| [role-rgm](https://framagit.org/rgm-community/ansible/roles/role-rgm)                           | This role handles all the aspects of RGM installation, configuration and customization |




Playbook variables
------------------

### Essential RGM variables

| variable name                   | default     | used by roles | description |
|---------------------------------|-------------|---------------|-------------|
| snmp_ro_community               | *undefined* | snmp, rgm     | set the SNMP community (ro) |
| lvm_disk                        | *undefined* | lvm-mount     | a complex data structure that describes how to prepare LVM volumes. see lvm-partitionner Ansible role [description](https://framagit.org/rgm-community/ansible/roles/role-lvm-partitionner) for details |
| rgm_root_path                   | /srv/rgm    | rgm           | root path for RGM installation |
| grafana_install_plugins_core    | True        | rgm           | installs Grafana's official plugins selection |
| grafana_install_plugins_contrib | False       | rgm           | installs Grafana's 3rd party plugins selection |
| proxy_host                      | *undefined* | rgm           | if a HTTP proxy is required for outgoing HTTP/HTTPS connections, defines the proxy's IP address |
| proxy_port                      | *undefined* | rgm           | if a HTTP proxy is required for outgoing HTTP/HTTPS connections, defines the proxy's TCP port |

Many other variables exists and can be tweaked to fit your needs, altrought you probably shouldn't have to. For more information,
please have a look on rgm role [README](https://framagit.org/rgm-community/ansible/roles/role-rgm)


### 4any related variables

Note that all of these variables are **mandatory** in the context of a 4any instance and must be defined !

| variable name                   | default     | used by roles | description |
|---------------------------------|-------------|---------------|-------------|
| rgm_central_host                | *undefined* | rgm           | Specifies the RGM central monitoring instance for 4any infrastructures |
| xinetd_livestatus_port          | 14176       | rgm           | Specifies the port to use for livestatus stream connection between 4any's RGM instance and central RGM instance |
| forany_rgmapi_username          | *undefined* | rgm           | the username used by 4any orchestrator to connect to RGM's API for manipulating monitoring resources |
| forany_rgmapi_password          | *undefined* | rgm           | the corresponding password for RGM API |
