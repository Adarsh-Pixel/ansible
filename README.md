# ansible

Ansible is a configuration management tool which works both on push and pull mechanism.

# INVENTORY FILE

----
 Inventory is nothing but a list of machine ip or dns names that you want ansible to be managed
 ----

Ansible by default installs 2.9 as by default on AMI's we will get python2.
Always go with latest version, which can be installed from tools/ansible/install.sh

'all' is a group which included all the entries in INVENTORY File

### Ansible is all about Modules (Collections)

ANSIBLE has lot of pre-defined variables and we need to use them to supply userName and password it has to use to authenticate to the nodes

### ansible_user        : predefines variable for userName
### ansible_password    : predefined variable for password

Variables should be passed to ansible by using the flag -e

----
    Ex: ansible -i INVENTORY all -e ansible_user=userName -e ansible_password=password 
    $ ansible -i inv all -e ansible_user=centos -e ansible_password=xyz123 -m shell -a uptime  ## -m:module -a: argument
----

## audomated Approach : This uses Playbook

...
Playbooks are written using a language called YAML.

YAML is just markup language : Markup language is nothing but a presentation language
....

*YAML* is indendation specific

### What is a playbook ?

---
* Playbook : A Playbook is a list of plays ( and that's why it always starts with -)
* Play     : A Play is a list of tasks.
* Task     : A Task is nothing but an action that we wish to perform

# what is a fact?
In ansible, fact is the property of the node mentioned in the inventory file. By default, ansible is going to gather all the facts the all the machines mentioned in the inventory file.
---
    $ ansible -i inv all  -e ansible_user=centos -e ansible_password=DevOps321 -m setup
---

## V.*IMP

# Note: If you would just like to print a variable then enclose the variable in "{{varName}}" & there is no single quote concept.
#        & if the variable is present in between the string of words, you don't have to enclose them in quotes.

#       1) No two tasks of a play can have the same name.



## Ansible Roles

---
roles/
    common/                     # This hirerachy represents a "role"
        tasks/
            main.yml            # <--- tasks file can include smaller files if warranted
        handlers/
            main.yml            # <-- handlers file
        templates/
            ntp.conf.js         # ,---- templates end in .j2
        files/
            bar.txt             # <-- files for use with the copy resource
            foo.sh              # <--- script files for use with the script resoruce
        vars/
            main.yml            # variables associated with this role
        defaults/
            main.yml            # <--- default lower priority variable for this role
        meta/
            main.yml            # <--- role dependencies
---

## When to use ansible pull vs ansible push

---
1) When your infrastructure is static, then we will host an ANSIBLE server and will target configuration management on all the nodes from your ansible server.

2) when your infrastructure is not static, which means on cloud we often scale out and down the infra, in this case maintaining the inventory is quite challenging and to avoid this, what we do is, as a part of the BOOTSTRAPPING, we will let the ansible pull command to run.

    Points to be noted when using pull: 
        * ensure your nodes running ansible has ansible installed.
        * Pull only works from GIT.

-----
### How to use ansible pull?

---
    $ ansible-pull -U https://github.com/Adarsh-Pixel/ansible.git -e ENG=dev -e COMPONENT=mongodb  roboshop-pull.yml
---

# Role Dependency

In Ansible terms, a dependency is any role that needs to have run before the current role runs.

### What is a handler and why do we need that ?

--- 
Ansible provides feature like handlers, which is like a task but only run when called by a notifier in another task.

This feature is important because your requirements for running a task may depend on the state of a service, existance of a file or a followup tasks when state changed.
---