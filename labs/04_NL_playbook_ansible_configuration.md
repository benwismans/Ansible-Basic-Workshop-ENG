# Lab 4: Playbook - Workshop voorzetten vanaf de Client
In dit lab gaan we de workshop files overzetten naar de Client, zodat de rest van de workshop vanaf de Client uitgevoerd kan worden.

## Task 4.1: Workshop files overzetten
Om de workshop files over te zetten, maken we gebruik van de copy module om de files naar de Client te kopieëren.
  
* Vul je playbook aan met:

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

**Tip:** Met ``with_items`` kun je een lijst genereren. Ansible vult dan steeds de variable ``item`` met de onderdelen uit de lijst.

## Task 4.2: SSH key overzetten
De laatste stap is het overzetten van de SSH key. 

* Vul je playbook aan met:

  ```
  - name: "Ensure SSH key is installed on the Client"
    copy:
      src: "~/.ssh/{{ item }}"
      dest: "/home/userXX/.ssh/{{ item }}"
      owner: "userXX"
      group: "userXX"
      mode: "0600"
    with_items:
    - id_rsa
    - id_rsa.pub
  ```

* Start het playbook. Als alles goed is gegaan, is nu Ansible start-klaar op je Client!

  ``$ ansible-playbook workshop.yml``

## Task 4.3: Werking testen
Als het playbook alleen nog maar "ok" meldingen geeft, is het tijd om in te loggen op de Client, om te controleren of Ansible werkt.

```
PLAY RECAP ****************************************************************************************************************************
client                         : ok=6    changed=0    unreachable=0    failed=0
```

* Log in op je Client (vervang ``<hostname>`` met de hostname van je client):

  ``$ ssh -l userXX <hostname>``
  
* Controleer de versie van Ansible (2.9.0):

  ``$ ansible --version``
  
  ```
  ansible 2.9.0
    config file = /home/user11/ansible.cfg
    configured module search path = [u'/home/user01/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
    ansible python module location = /usr/lib/python2.7/site-packages/ansible
    executable location = /usr/bin/ansible
    python version = 2.7.5 (default, Apr  2 2020, 13:16:51) [GCC 4.8.5 20150623 (Red Hat 4.8.5-39)]

  ```

* Test of het ``adhoc`` commando ``ping`` werkt:

  ``$ ansible -m ping workshop``
  
  ```
  client | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
  }
  ```

* Voer het playbook uit:

  ``$ ansible-playbook workshop.yml``
   
Volgende stap: [Lab 5 - Role - User aanmaken](/labs/05_NL_role_user.md)
