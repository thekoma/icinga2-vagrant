# icinga2-vagrant

## Requirements:

You need Ansible Core 2.11 or later (Ansible 4.5).<br>
The main problem is the structure of ansible-galaxy


```bash
git clone https://github.com/thekoma/icinga2-vagrant
cd icinga2-vagrant
python3 -m venv ansible
source ansible/bin/activate
pip3 install --upgrade pip
pip3 install --upgrade ansible


```
## Customization:
To customize the playbook edit `src/extra_vars.yaml `.

## Defaults:
```yaml
---
# default vars for icingadb role
icingaweb2_db: icingaweb2
icingaweb2_db_user: icingaweb2
icingaweb2_db_password: icingaweb2
icinga_ido_db: icinga2
icinga_ido_user: icinga2
icinga_ido_password: icinga2
icinga_director_db: icingadirector
icinga_director_db_user: icingadirector
icinga_director_db_password: icingadirector

---
# default vars for icinga2 role
icinga2_features:
  - name: checker
  # - name: command # Moved on API but here I wrote a patch https://github.com/Icinga/ansible-collection-icinga/pull/74
  - name: mainlog
  - name: notification
  - name: syslog
  - name: api
    ca_host: none
  - name: idomysql
    host: localhost
    port: 3306
    database: "{{ icinga_ido_db }}"
    user: "{{ icinga_ido_user }}"
    password: "{{ icinga_ido_password }}"
    import_schema: true
    cleanup:
      downtimehistory_age: 48h
      contactnotifications_age: 31d

---
# default vars for icingaweb role
icinga_monitoring_module_apiuser: monitoring-module
icinga_monitoring_module_apiuser_password : youreallyneedtochangeme
icinga_director_apiuser: director-module
icinga_director_apiuser_password: youreallyneedtochangeme
icingaweb_admin: admin
icingaweb_admin_password: password

director_repo: "https://github.com/icinga/icingaweb2-module-director"
director_version: "1.8.1"

incubator_repo: "https://github.com/icinga/icingaweb2-module-incubator"
incubator_version: "0.6.0"
```
