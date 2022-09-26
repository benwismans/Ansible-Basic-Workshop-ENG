# Lab 4: Playbook - Copy Items
In this lab we will copy files to the client system.

## Task 4.1: Workshop files 
To copy the workshop files, we will use the ``copy`` module.
  
* Add to your existing playbook (remember user number):

  ```
  - name: "Ensure Ansible workshop files are copied to the Client"
    copy:
      src: "{{ item }}"
      dest: "/home/userXX/{{ item }}"
      owner: "userXX"
      group: "userXX"
    with_items:
    - ansible.cfg
    - inventory
    - workshop.yml
  ```

**Tip:** The ``with_items`` is used to generate a list. Ansible repeatedly fills the variable ``item`` with the value in the list (a for loop).

## Task 4.2: SSH key 
The last step is copyin the SSH keypair to the 2nd system. 

* Add to your playbook:

  ```
  - name: "Ensure SSH key is installed on the Client"
    copy:
      src: "~/.ssh/{{ item }}"
      dest: "/home/user02/.ssh/{{ item }}"
      owner: "user02"
      group: "user02"
      mode: "0600"
    with_items:
    - id_rsa
    - id_rsa.pub
  ```

* Start the playbook. 

  ``$ ansible-playbook workshop.yml``

## Task 4.3: Werking testen
If the playbook only returns "ok" results, it is time to login to the Client and check if the files are present.

```
PLAY RECAP ****************************************************************************************************************************
client                         : ok=6    changed=0    unreachable=0    failed=0
```

* Log in on the second system and check if the files are there (replace ``<hostname>`` with the hostname of your client):

  ``$ ssh -l user02 <hostname>``
  
  
   
Next step: [Lab 5 - Role - User](/labs/05_NL_role_user.md)
