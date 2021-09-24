# Introductie en Voorbereidingen
Welkom bij de workshop Ansible-Basic. In deze workshop gaan we een  VM met Linux inrichten als Ansible Server.  

## Benodigdheden
Voor deze workshop heb je nodig:
- Laptop met een SSH client (bijvoorbeeld putty: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- Een Linux server met SSH toegang
- Lokale user op de VM
- Root rechten op de VM

## Uitvoering
- Alle acties worden uitgevoerd vanuit de home directory van de SSH server (tenzij anders aangegeven).
- Instructies voor commando's worden vooraf gegaan met een prompt teken ($) en staan in een tekst blok. Dit teken is onderdeel van de opdracht prompt en hoort niet bij het commando (wie kent de dos prompt ``C:\>`` nog?). Bijvoorbeeld (Het commando in het voorbeeld is dus ``ls``):

  ``$ ls``
  
- De output wordt ook in een tekstblok weergegeven. Bijvoorbeeld:
  ```
  pi@raspberry:~ $ ls -l
  total 3
  -rw-r--r-- 1 pi pi   84 Feb 14 13:30 ansible.cfg
  -rw-r--r-- 1 pi pi  979 Feb 14 17:07 brocade.yml
  -rw-r--r-- 1 pi pi  884 Feb 14 22:07 cisco.yml
  ```
  
  ## Installatie Ansible
- Deze acties moeten als root, of via een user die via sudo rechten heeft uitgevoerd worden (gebaseeerd op CentOS/RHEL, voor Oracle Linux zie onder):
```
user@vm:~ $ sudo yum install epel-release
user@vm:~ $ sudo yum install ansible
user@vm:~ $ ssh-keygen (nodig voor latere opdracht, bij voorkeur niet als root user uitvoeren)

- Oracle Linux X (voor OL8 verander je ol7_developer_EPEL in ol8_developer_EPEL):
user@vm:~ $ sudo yum-config-manager --enable ol7_developer_EPEL
user@vm:~ $ sudo yum install ansible-python3

```


  
Volgende stap: [Lab 1 - Inventory file aanmaken](/labs/01_NL_inventory.md)
