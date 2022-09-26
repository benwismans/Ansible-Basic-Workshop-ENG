# Lab 5: Role - User 
For almost every challenge there is a role already created on Ansible Galaxy. The disadvantage of this is that there are too many, which makes it difficult to find the good stuff. You can check the number of downloads and the ranking (in stars). You should always check the source code of a role you found on Ansible Galaxy to make sure it does what you want it to do. You can always adjust things if needed.

Almost all roles use variables. These are often described in the documentation of the role. The variables can be defined inside the role in the directory ``defaults`` and in the directory ``vars``.

For creating a user, you can search for ``accounts``. The role ``ontic.account`` (https://galaxy.ansible.com/ontic/account) seems to do exactly what we would want in our lab. We are going to install the role using Ansible Galaxy. 

**Tip** Next to looking at the stars of downloads you can also check if a developer built multiple roles. The ontic roles might not have a lot of stars, but the developer made dozens of roles. Some well known contributors are:
* https://galaxy.ansible.com/debops
* https://galaxy.ansible.com/geerlingguy
* https://galaxy.ansible.com/Oefenweb

## Task 5.1: Role installation

We are going to use an Ansible Role to create a user account. Because the tasks are already un the role, we only need to install the role and adjust the parameters.

* Install the role via Ansible Galaxy:

  ``$ ansible-galaxy install ontic.account``

The role will be installed in the ``.ansible/roles`` directory in your home directory. 

* Check if the role is installed in ``.ansible/roles``:

  ``$ ls ~/.ansible/roles``
  
  ```
  ontic.account
  ```  

* Inspect the role:

  ``$ cd ~/.ansible/roles/ontic.account``
  
  ``$ ls``
  
  ```
  defaults  docs  meta  tasks  tests
  ```

A description of the ports of a role can be found in the documentation of Ansible on: https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html.

**Tip:** The remaining part of this workshop will be executed from the homedirectory of the user. So go back with the command ``$ cd``

## Task 5.2: Password hash generation
The ``user`` module expects the password in SHA512 format. 

* You can use Ansible to generate a SHA512 hash (replace ``<WorkshopPassword>`` with your own password):

  ``$ ansible localhost -m debug -a "msg={{ '<WorkshopPassword>' | password_hash('sha512') }}"``

  ```
  localhost | SUCCESS => {
    "msg": "$6$uj/GXuBze4eetOeT$ksVseNMTnsRdkVFqUyTICzxri9TeRnsqJyUZVRiiy6ChlDurXWsTkAOdPuSNOPJtPNnzkmrXzfx753hglmH5M/"
  }
  ```

## Task 5.3: Create a playbook with a role

* Create a new playbook ``workshop-role.yml`` (watch out: replace the  hash with the hash from task 5.2):

```
---
- hosts: workshop
  become: true
  become_method: sudo

  vars:
    account_groups:
    - name: "workshop"
    account_users:
    - name: "workshop"
      password: "$6$uj/GXuBze4eetOeT$ksVseNMTnsRdkVFqUyTICzxri9TeRnsqJyUZVRiiy6ChlDurXWsTkAOdPuSNOPJtPNnzkmrXzfx753hglmH5M/"

  roles:
  - role: ontic.account
```

* Run the playbook:

  ``$ ansible-playbook workshop-role.yml``
  
* Check if the user is created with your password (replace ``<hostname>`` by the hostname of your Client):

  ``$ ssh -l workshop <hostname>``
  
  ```
  workshop@client password: 
  $
  ```

# Example
In production ansible environments you will place the variables in files that you use in a playbook. (see: https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html). For webservers you could create a ``webserver_vars.yml``, with a defenition of the ``accounts``, ``databases`` and ``virtual_hosts``. The playbook would then exist of 3 roles: ``accounts``,``mysql`` and ``apache``.

Below is just an example, and does not need to be run: 

**webserver_vars.yml**:

```
---
account_groups:
  - name: "workshop"
account_users:
  - name: "workshop"
    password: "$6$uj/GXuBze4eetOeT$ksVseNMTnsRdkVFqUyTICzxri9TeRnsqJyUZVRiiy6ChlDurXWsTkAOdPuSNOPJtPNnzkmrXzfx753hglmH5M/"

mysql_root_password: root-password"
mysql_users:
  - name: 'deployer'
    host: '%'
    password: 'strong-password'
    priv: 'my-database.*:ALL'
  - name: 'oldclient'
    state: 'absent'
mysql_databases:
  - name: 'application_database'
    encoding: 'utf8'
    collation: 'utf8_general_ci'
  - name: 'old_database'
    state: 'absent'

apache_vhosts:
  - servername: "local.dev"
    documentroot: "/var/www/html"
```

**playbook.yml**:

```
- hosts: webservers
  vars_files:
    - webserver_vars.yml
  roles:
    - role: accounts
    - role: mysql
    - role: apache
```

The variable file describes how the webservers should look and the playbook gets more organized. 
  
Next step: [Lab 6 - Vault - Encryption](/labs/06_NL_vault.md)
