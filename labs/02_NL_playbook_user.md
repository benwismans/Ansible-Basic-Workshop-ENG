# Lab 2: Playbook - User configureren
Een playbook is een beschrijving hoe een systeem ingericht zou moeten zijn. Dit playbook bestaat uit een lijst met stappen. Elke stap controleert de huidige toestand en past deze, indien nodig, aan. Komt het systeem al overeen met de beschrijving van de stap, dan doet Ansible niets.

**Tip:** Probeer het playbook zo in te richten dat er geen changes meer worden gemaakt, wanneer het playbook voor de 2e maal uitgevoerd wordt. Daarnaast is het gebruikelijk om een playbook zodanig te maken dat deze meerdere malen uitgevoerd kan worden, zonder dat dit problemen geeft.

In dit lab maken we een playbook voor het installeren van de public key voor SSH. Na het installeren van deze public key kun je met de private key (dus zonder wachtwoord) inloggen op de Client.

**Tip:** In een productie omgeving levert het een beveiligings risico op, wanneer er geen Passphrase is geconfigueerd op de Private key. Het is daarom niet aan te raden om Private keys zonder een Passprhase te gebruiken in een productie omgeving. Om dit lab minder complex te maken, is de Passphrase voor de Private key leeg gelaten.

## Task 2.1: Playbook aanmaken
In het playbook gaan we de module ``authorized_key`` gebruiken om de SSH public key op de Client te installeren. In de verborgen directory .ssh is de public en de private key voor geïnstalleerd.

* Controleer of de SSH keys zijn geïnstalleerd (zie stap 0, voorbereidingen):

  ``$ ls ~/.ssh/``

  ```
  id_rsa  id_rsa.pub
  ```

De file ``id_rsa.pub`` is de public key. De file ``id_rsa`` is de private key. Met het commando ``ssh-keygen`` kun je de keys opnieuw genereren. Let er wel op dat de oude keys dan overschreven worden. De nieuwe private key kan niet gebruikt worden op systemen die nog de oude public key hebben.
 
* Maak het playbook aan:

  ``$ vi workshop.yml``
  
* Vul het playbook met (let weer op je usernummer!):

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

**Tip:** Playbooks werken met Yaml files. Voor de werking van Yaml files is het belangrijk dat het inspringen van de regels nauwkeurig gebeurd. Het is gebruikelijk om dit met 2 (of 4) spaties te doen. Als je ooit met Python hebt gewerkt, dan zul je dit herkennen. 

Als het goed is, valt op dat het playbook redelijk leesbaar is. Zelfs zonder kennis van Ansible is redelijk in te schatten wat dit playbook uit zal voeren. De samenvatting:
* Het playbook zal uitgevoerd worden op alle clients in de groep ``workshop``.
* Het playbook bestaat uit een enkele taak.
* Met ``name`` wordt beschreven wat deze taak doet.
* De module ``authorized_key`` wordt gebruikt om voor de ``user`` ``userXX`` de ``key`` te installeren. Daarbij wordt de file ``~/.ssh/id_rsa.pub`` gebruikt.

In de documentatie vind je meer details over de module ``authorized_key``. Zie https://docs.ansible.com/ansible/latest/modules/authorized_key_module.html.

## Task 2.2: Het playbook starten

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

## Task 2.3: Het playbook nogmaals starten
Omdat nu de Authorized key voor SSH op de Client is geïnstalleerd, kun je zonder wachtwoord inloggen op de Client.

* Controleer of je zonder wachtwoord in kunt loggen (vervang ``<hostname>`` met de hostname van je client):
  
  ``$ ssh -l user02 <hostname>`` 

  ``` 
  userXX@client:~ $ 
  ```

* Log direct weer uit met ``exit``:

  ``userXX@client:~ $ exit``

  ```
  logout
  Connection to client closed.
  ```

* Pas de ansible.cfg aan, zodat je met de ansible-user inlogt. 
``$ vi ansible.cfg en pas de lokale user aan naar user02``

* Start het playbook, maar zonder de parameter ``--ask-pass``:

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

Het playbook zal nu geen changes opleveren. De public key is immers al geïnstalleerd. Mocht je later de public key willen vervangen, kun je simpelweg een nieuwe genereren en deze via Ansible opnieuw deployen. Ansible herkent dat het bestand is gewijzigd en zal daarvoor een change genereren.

Volgende stap: [Lab 3 - Playbook - Installatie Ansible](/labs/03_NL_playbook_ansible_installation.md)
