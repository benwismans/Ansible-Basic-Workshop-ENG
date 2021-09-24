# Lab 4: Playbook - Workshop voorzetten vanaf de Client
In dit lab gaan we de workshop files kopieren naar de Client.

## Task 4.1: Workshop files overzetten
Om de workshop files over te zetten, maken we gebruik van de copy module om de files naar de Client te kopieëren.
  
* Vul je playbook aan met:

  ```
  - name: "Ensure Ansible workshop files are copied to the Client"
    copy:
      src: "{{ item }}"
      dest: "/home/user02/{{ item }}"
      owner: "user02"
      group: "user02"
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
      dest: "/home/user02/.ssh/{{ item }}"
      owner: "user02"
      group: "user02"
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

* Log in op je Client en kijk of de bestanden er staan (vervang ``<hostname>`` met de hostname van je client):

  ``$ ssh -l user02 <hostname>``
  
  
   
Volgende stap: [Lab 5 - Role - User aanmaken](/labs/05_NL_role_user.md)
