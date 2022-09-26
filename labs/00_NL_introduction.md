# Introduction and Preparations
Welcome to the Ansible-Basic Workshop. In this workshop we will configure a Linux VM using Ansible.

## Requirements
For this workshop, you will need:
- Laptop with a SSH client (for example putty: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- A Linux Server with SSH connection
- Local user on the VM
- Root permissions on the VM

## Execution
- All actions will be done from the user's home directory on the 1st VM (unless otherwise mentioned)
- Instructions for commands will be preceded with a promot sign ($) and in a text block. This sign is for of the prompt and does not belong to the actual command. (The command in the example is ``ls``):

  ``$ ls``
  
- The output is alsow shown in a text block. For example:
  ```
  user@vm01:~ $ ls -l
  total 3
  -rw-r--r-- 1 pi pi   84 Feb 14 13:30 ansible.cfg
  -rw-r--r-- 1 pi pi  979 Feb 14 17:07 brocade.yml
  -rw-r--r-- 1 pi pi  884 Feb 14 22:07 cisco.yml
  ```
  
 # Pre-check SSH-keys
 You have 2 Linux VM's. The first VM already has the Ansible Server Core installed. There is a local user prepared for you on both VM's. In a later stage, you need to be able to login between the VM's without password. Therefor you need to generate a public/private SSH-keypair. This is possible with the command "ssh-keygen -t rsa". Do not put a passphrase on it.
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


  
Next step: [Lab 1 - Inventory File Creation](/labs/01_NL_inventory.md)
