# About This Repo #
This is a skeleton of an Ansible home directory. You can control all of your application environments from this directory. If you work with multiple customers, you probably want to create a separate copy of this repo for each customer.

This repo was designed to help new Ansible users to get up and running quickly. Feel free to add to it and add additional tips and tricks. Pull requests welcome.

# Ansible Tips #
In your roles directory, type ansible-galaxy init <em>role_name</em> in order to generate an empty skeleton for a new role you are working on.

Example:
<pre>
ansible-galaxy init nginx
</pre>

Check out [Ansible Examples](https://github.com/ansible/ansible-examples) for example playbooks.

# Best Practices Directory Layout #

Based on [Ansible Best Practices](https://docs.ansible.com/ansible/playbooks_best_practices.html#directory-layout)

<pre>
production                # inventory file for production servers
staging                   # inventory file for staging environment

group_vars/
   group1                 # here we assign variables to particular groups
   group2                 # ""
host_vars/
   hostname1              # if systems need specific variables, put them here
   hostname2              # ""

library/                  # if any custom modules, put them here (optional)
filter_plugins/           # if any custom filter plugins, put them here (optional)

site.yml                  # master playbook
webservers.yml            # playbook for webserver tier
dbservers.yml             # playbook for dbserver tier

roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""
</pre>

# Install a Galaxy Role
```
ansible-galaxy install --roles-path roles nickjj.user
ansible-galaxy install --roles-path roles geerlingguy.docker
```

# Test Ansible Setup

## Ping
```
ansible -i staging all -m ping --verbose --user root
```

## Uptime
```
ansible -i staging all -a uptime --verbose --user root
```

# Call a playbook with tags
```
ansible-playbook -i staging  site.yml -t common,user
```



# Hetzner - hcloud #

Some useful hcloud  commands

Server list: 
```bash
hcloud server list
```

Image list:
```bash
hcloud image list
```

server-type list:
```bash
hcloud server-type list
```

Rebuild all server
```bash
    for x in n1.k8s-monkey.ch n2.k8s-monkey.ch n3.k8s-monkey.ch; hcloud server rebuild  $x --image centos-7; end
```

Copy keys
```bash
    for x in n1.k8s-monkey.ch n2.k8s-monkey.ch n3.k8s-monkey.ch; ssh-copy-id -i ~/.ssh/id_rsa_hetzner root@$x; end
```

Reset 

```bash
    for x in n1.k8s-monkey.ch n2.k8s-monkey.ch n3.k8s-monkey.ch; hcloud server reset $x; end
```


List availible key
```
ls -al ~/.ssh
```

List SSH Key
```
ssh-add -l
```

Generating a new SSH key
```bash
ssh-keygen -t rsa -b 4096 -C "your@email-address" -f ~/.ssh/id_rsa_hetzner
```

Add SSH key to agent 
```bash
ssh-add ~/.ssh/id_rsa_hetzner
```

Create SSH key in hetzner cloud
```bash
hcloud ssh-key create --name k8s-monkey --public-key-from-file ~/.ssh/id_rsa_hetzner.pub
```

Create Server
```bash
hcloud server create --name cimonkey --type cx11 --image centos-7 --ssh-key k8s-monkey
```


```bash
ssh root@remote-ip
```



