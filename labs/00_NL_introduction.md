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
  user@vm01:~ $ ls -l
  total 3
  -rw-r--r-- 1 pi pi   84 Feb 14 13:30 ansible.cfg
  -rw-r--r-- 1 pi pi  979 Feb 14 17:07 brocade.yml
  -rw-r--r-- 1 pi pi  884 Feb 14 22:07 cisco.yml
  ```
  
 # Pre-check SSH-keys
 Je heb 3 Linux VM's. Op de 1e gaan we Ansible Server installeren. Als het goed is, is er een lokale user waarmee we ingelogd zijn en waarmee we naar de andere 2 VM's straks moeten kunnen connecteren. Deze lokale user moet een public/private SSH-key hebben. Deze is te maken met "ssh-keygen -t rsa". Zet er geen passphrase op.
   ```
user@vm01:~ $ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa):
Created directory '/home/user/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/user/.ssh/id_rsa.
Your public key has been saved in /home/user/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:9sgRPo0fXQ+keNdNgCuJFeT0Wttz2vorfa6cDdxA/PE user@vm01
The key's randomart image is:
+---[RSA 3072]----+
|         .+. .o..|
|         o.o.+ o.|
|        .oo.=.*.o|
|       ..+o=.* +o|
|        S +.o + E|
|       o * . . B |
|        o o   = o|
|             o *.|
|              *=*|
+----[SHA256]-----+
user@vm01:~ $ ls -a .ssh
.  ..  id_rsa  id_rsa.pub
user@vm01:~ $ 
```

  
 ## Installatie Ansible
- Deze acties moeten als root, of via een user die via sudo rechten heeft uitgevoerd worden (gebaseeerd op CentOS/RHEL, voor Oracle Linux zie onder):
```
user@vm01:~ $ sudo yum install epel-release
user@vm01:~ $ sudo yum install ansible
user@vm01:~ $ ssh-keygen (nodig voor latere opdracht, bij voorkeur niet als root user uitvoeren)

- Oracle Linux X (voor OL8 verander je ol7_developer_EPEL in ol8_developer_EPEL):
user@vm01:~ $ sudo yum-config-manager --enable ol7_developer_EPEL
user@vm01:~ $ sudo yum install ansible-python3

```


  
Volgende stap: [Lab 1 - Inventory file aanmaken](/labs/01_NL_inventory.md)
