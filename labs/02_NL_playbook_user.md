# Lab 2: Playbook - User configuration
A playbook is a description of how a system should be configured. This playbook contains a list of steps. Every step checks the current condition and adjusts it, if necessary. If the system already matches the description of the step, Ansible does nothing. We call this idempotent.

**Tip:** Try to create your playbook so no changes are made anymore, when the playbook is run for the 2nd time. It's usual to run a playbook multiple times without giving any issues.

In this lab we create a playbook for installing the public key for SSH. After the installation of the public key, you can logon with your private key (without password) on the client system.

**Tip:** In a production environment logging in with a private key without passphrase would mean a security risk. It's not advisable to use this setup in a production environment. But to make this lab less complex, we decided to do it anyway. In a production ansible environment, you can encrypt your passwords and passphrases with ansible vault (shown in a later lab) or ansible tower (which is also one big vault).

## Task 2.1: Playbook creation
In the playbook we will use the module ``authorized_key`` to install the SSH public key on the Client. The .ssh directory contains the private and public key created in Lab 0.

* Check if the SSH keys are installed (see lab 0, preparations):

  ``$ ls ~/.ssh/``

  ```
  id_rsa  id_rsa.pub
  ```

The file ``id_rsa.pub`` is the public key. The file ``id_rsa`` is the private key. 
 
* Create the playbook:

  ``$ vi workshop.yml``
  
* Add to your playbook (watch your usernumber!):

  ```
  ---
  - hosts: workshop

    tasks:
    - name: "Ensure authorized key is installed for user"
      authorized_key:
        user: userXX
        state: present
        key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
  ```

**Tip:** Playbooks work with Yaml files. Yaml works with indents of 2 spaces. If you ever worked with python, you will recognize this.

The playbook should be fairly readble. Even without Ansible knowledge, you should be able to guess what this playbook will do. The summary:
* The playbook will be executed on all clients in the group ``workshop``.
* Het playbook contains one single task.
* After ``name`` is a description of the task.
* Te module ``authorized_key`` is used to install  the ``key`` for ``user`` ``userXX``. The file ``~/.ssh/id_rsa.pub`` is used for this.

In the documentation you can find more details about the module ``authorized_key``. See https://docs.ansible.com/ansible/latest/modules/authorized_key_module.html.

## Task 2.2: Start the playbook

  ``$ ansible-playbook --ask-pass workshop.yml``

  ```
  SSH password:

  PLAY [workshop] **********************************************************************************************************************************************************************************************************

  TASK [Gathering Facts] ***************************************************************************************************************************************************************************************************
  ok: [client]

  TASK [Ensure authorized key is installed for user user09] ********************************************************************************************************************************************************************
  changed: [client] 

  PLAY RECAP ***************************************************************************************************************************************************************************************************************
  pi                         : ok=2    changed=1    unreachable=0    failed=0
  ```

## Task 2.3: Start the playbook again
Because the authorized key is installed, you can login to the system without password.

* Check the connection without password (replace ``<hostname>`` with the hostname of your client):
  
  ``$ ssh -l user02 <hostname>`` 

  ``` 
  userXX@client:~ $ 
  ```

* Log out again with ``exit``:

  ``userXX@client:~ $ exit``

  ```
  logout
  Connection to client closed.
  ```

* Start the playbook, but without the parameter ``--ask-pass``:

  ``$ ansible-playbook workshop.yml``
  
  ```

  PLAY [workshop] **********************************************************************************************************************************************************************************************************

  TASK [Gathering Facts] ***************************************************************************************************************************************************************************************************
  ok: [client]

  TASK [Ensure authorized key is installed for user user02] ********************************************************************************************************************************************************************
  ok: [client] => (item=ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCnZtlzLhYrZAIxTiiN/b5WaRAHaze4BecufyjpQkQ9QCSqglfxnKSERtrwQmes31FJPRNY2DWvzvSgV1cJHnyYWKFeWQJv6nVvSCFOpmtqbqPHuSVV1O5S3CLHrmLWtZ8CeBNawnAMBlaDzZ2h9duDED+Ecx/bYYJakcQXR++LpqQ1voYX8gwGLD8dBY3i+hgjZ/pA6ITM1PLVwNaHzUZ5uL3ne6/RyzsjCfK+cJdxt+OtN6QsGHJwrV3hX3mVcyZVE3Ta72/1asm3CzeQAYA3CwBdxqfAONYck8UZeh8N0VtTsX+g8nrPBozRv47nF4JhFjBG2N/u37MEixoN8skV user@host)

  PLAY RECAP ***************************************************************************************************************************************************************************************************************
  client                         : ok=2    changed=0    unreachable=0    failed=0
  ```
The playbook will have no changes, as the public key is already installed. If you want to replace the public key later, all you have to do is generate a new one, and run the playbook. Ansible will notice the difference in the files and will change the file.

Next step: [Lab 3 - Playbook - Install Ansible](/labs/03_NL_playbook_ansible_installation.md)
