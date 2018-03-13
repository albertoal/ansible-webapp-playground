# Ansible NginX webapp playground
I wanted to play with Ansible so I've created playbook that will deploy a simple NginX webapp similar to [albertoal/codecore--ec2-demo](https://github.com/albertoal/codecore--ec2-demo) but with Ansible instead of ec2 user-data
Disclaimer: This has only be tested with Ubuntu 16 and I've used Ansible v2.4.3


## User Inputs

  - deploy.yml needs the path to your ssh key. You have alternate methods in order to do this [here](https://stackoverflow.com/questions/33795607/how-to-define-ssh-private-key-for-servers-fetched-by-dynamic-inventory-in-files)
  - hosts/aws needs the IP or IP's of your remote hosts

## Usage

```bash
ansible-playbook deploy.yml -i hosts/aws
```

## Repo layout

```
|-- README.md 
|-- deploy.yml #Main playbook file
|-- hosts
|   `-- aws #Inventory file with the list of hosts where this playbook should run 
`-- roles 
    `-- nginx #nginx role directory
        |-- defaults #In here I defined a few variables such as source and destination copy directories
        |   `-- main.yml
        `-- tasks #Tasks that will be executed by the playbook and will perform the actual deployment
            `-- main.yml
```

## Output

You can add more verbose output with -v or using the Ansible Playbook Debugger. This is how the output should hopefully look like on the first run: 

```
PLAY [web] ************************************************************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************************************************************
The authenticity of host '54.186.127.29 (54.186.127.29)' can't be established.
Are you sure you want to continue connecting (yes/no)? yes
ok: [54.186.127.29]

TASK [nginx : Update repos cache] *************************************************************************************************************************************************************
changed: [54.186.127.29]

TASK [nginx : Install Git package] ************************************************************************************************************************************************************
ok: [54.186.127.29]

TASK [nginx : Install Nginx package] **********************************************************************************************************************************************************
changed: [54.186.127.29]

TASK [nginx : Clone Git repo] *****************************************************************************************************************************************************************
changed: [54.186.127.29]

TASK [nginx : Create new website root directory] **********************************************************************************************************************************************
changed: [54.186.127.29]

TASK [nginx : Copy index.html file and NginX server block files] ******************************************************************************************************************************
changed: [54.186.127.29] => (item={u'dest': u'/var/www/codecore.io/index.html', u'src': u'/home/ubuntu/ec2-demo/index.html'})
changed: [54.186.127.29] => (item={u'dest': u'/etc/nginx/sites-available/codecore.io', u'src': u'/home/ubuntu/ec2-demo/codecore.io'})

TASK [nginx : Create symbolic link on sites-enabled] ******************************************************************************************************************************************
changed: [54.186.127.29]

TASK [nginx : Unlink default NginX site from sites-enabled] ***********************************************************************************************************************************
changed: [54.186.127.29]

TASK [nginx : NginX service is restarted] *****************************************************************************************************************************************************
changed: [54.186.127.29]

PLAY RECAP ************************************************************************************************************************************************************************************
54.186.127.29              : ok=10   changed=8    unreachable=0    failed=0
```