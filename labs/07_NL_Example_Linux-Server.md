# Lab 7: Extra - Apache Servers
In this extra lab we will install apache on a linux server. We will also use tags.
  
## Task 7.1: Apache installeren

The first step is to install the webserver application. After the installation the webserver needs to be started. 

* Create the playbook ``linux.yml``:

  ```
  ---
  - hosts: workshop
    become: true
    become_method: sudo

    tasks:
      - name: Install apache packages
        yum:
          name: httpd
          state: present
        tags: install

      - name: ensure httpd is running
        service:
          name: httpd 
          state: started
        tags: install
  ```

* Run the playbook:

  ``$ ansible-playbook linux.yml``
  
## Task 7.2: Firewall configuration

The properly use the webserver, we need to open port 80 (http) for TCP traffic. We use the Ansible module ``firewalld``. To be sure that the new rule is enforced, we restart the systemd service ``firewalld`` after the change. 
 
* Add to the playbook ``linux.yml``:
 
  ```
      - name: Ensure port 80 is open for http access
        firewalld:
          service: http
          permanent: true
          state: enabled
        tags: configure

      - name: Ensure firewalld service is restarted after the firewall changes
        service: 
          name: firewalld 
          state: restarted
        tags: configure
   ```

* Run the playbook again, but just with the configure tag:

  ``$ ansible-playbook linux.yml --tags configure``
   
## Task 7.3: Install content for the webserver

  
* Add to the playbook ``linux.yml``:

  ```
      - name: Ensure content is installed in the webserver
        copy:
          content: "Hello World! This is the Ansible Basic Workshop Extra Lab on the {{ ansible_facts.hostname }} Linux Server\n"
          dest: /var/www/html/index.html
          owner: apache
          group: apache
          mode: 0644
        tags: content
  ```
    
* Run the playbook with the content tag:

  ``$ ansible-playbook linux.yml --tags content``
  
* Open in your browser the url ``http://<hostname>.<domain-name>`` (change ``<hostname>`` by the  hostname of the Linux server).
