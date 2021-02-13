# satellite_ansible
Red Hat Satellite Installation with Ansible

Current use for this role is to install a basic satellite server. There 
is an option for toggling connected|disconnected depending upon the connectivity
available. 

Since version 2.9 ansible collections have been incorporated and this role is using them.

## Ansible Collections:
To pull red hat official ansible collections follow this guide: https://www.ansible.com/blog/hands-on-with-ansible-collections

To install and/or download the collections:
`ansible-galaxy collection install -r collections/requirements.yml`
`ansible-galaxy collection download -r collections/requirements.yml`

## How to use this role
```yaml
ansible-playbook -i inventory site.yml
```

## Assumptions
A control node with at least ansible 2.9
Imported ansible collections required:
- redhat.satellite
- community.general
- community.posix
- A separate disk is attached for satellite data (`example: /dev/sdb`)

## Before running the playbook:
Make sure that the satellite FQDN is resolvable either via DNS or /etc/hosts

### Variables
| Name | Value | Purpose |
|------|-------|---------|
|disconnected | false | true|false |
|redhat_subscription_pool_ids | <empty> | Subscription pool id |
|redhat_network_username | <empty> | RHN Username - when connected: true |
|redhat_network_password | <empty> | RHN Password - when connected: true |
|satellite_repos | rhel-7-server-rpms<br />rhel-7-server-satellite-6.8-rpms<br />rhel-7-server-satellite-maintenance-6-rpms<br />rhel-server-rhscl-7-rpms<br />rhel-7-server-ansible-2.9-rpms | Required Satellite Repositories for installation|
|satellite_packages | satellite<br/>chrony<br/>firewalld<br/>vim<br/> | Packages for Satellite installation |
|satellite_service | chronyd | Start and Enable list of services |
|satellite_firewall_rules|RH-Satellite-6|Add Red Hat Satellite6 service to firewalld|
|satellite_organization| <empty>| Set name for the satellite organization |
|satellite_location| <empty> | Set name for satellite location |
|data_disk|`/dev/sdb`|Disk to add to LVM for Satellite content|
|satellite_fqdn|<empty>|Set the fqdn: ex: `sat.example.com`|
|satellite_vg|`satellite_data`|Define the name of the volume group|
|satellite_data_lvm|satellite_psgql<br/>satellite_mongodb<br/>satellie_pulp_cache<br/>satellite_pulp<br/>|`25G`<br/>`50G`<br/>`300G`<br/>`100%FREE`| LVM name and size|
|product_repos|rhel-7-server-rpms<br/>rhel-7-server-satellite-6.8-rpms<br/>rhel-7-server-satellite-maintenance-6-rpms<br/>rhel-server-rhscl-7-rpms<br/>rhel-7-server-ansible-2.9-rpms<br/>rhel-7-server-ansible-2.8-rpms<br/>rhel-7-server-ose-3.11-rpms<br/>rhel-7-server-extras-rpms|Repos to sync when satellite is up|
|disconnected_content_server| <empty> | Content Source on disconnected side for provisioning|
|disconnected_yum_repos|see: `satellite_repos` entry| Configuring yum repo (/etc/yum.repos.d/content.repo) to provision satellite|
