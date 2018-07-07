# README


## Prereqs

```
sudo dnf install ansible libselinux-python -y
pip install awscli --upgrade --user
pip install boto --upgrade --user
pip install boto3 --upgrade --user
```

Assumes you have setup AWS CLI.

```
aws configure
```

## Create Infrastructure

```
ansible-playbook create.yml
```

Will create an inventory file `.inventory` for use with server setup.  If this file is deleted it will be recreated.

### Test Inventory File

```
ansible -i .inventory all -a "/bin/echo success"
```

## Teardown Instance

Only tears down the instance & EIP at this time.

```
ansible-playbook teardown.yml
```

## Setup Minecraft Server

```
ansible-playbook install.yml
```

## Setup Minecraft Client

Installs mods, same as server, and creates /tmp/index.html with some server info.

```
ansible-playbook client.yml
```


# Troubleshoot

## EC2 Instance ID
If you need to recreate your server you'll probably hit a problem of reusing the same `id` for the ec2 instance.  To get around this, edit `var/minecraft.yml` and change the value of `ec2.instance.id` to something that hasn't been used in the last 24 hours.

## Whitelist

If you don't want random people joining your server you can turn on whitelist and add only friends.  In the server as an op:

```
/whitelist on
/whitelist add <username>
```

## SSH Known Hosts

With a dynamic IP address you'll have to manually cleanup known_hosts, ssh to the IP, or configure ssh to not check host IP.  For example, in `~/.ssh/config`:

```
Host <your minecraft server DDNS entry>
        CheckHostIP no
        User ec2-user
        IdentityFile ~/.ssh/id_rsa_minecraft
```

## Namecheap

Obviously can't give you my namecheap stuff.  If you use namecheap you can leverage Dynamic DNS to update the instance when the public ip changes (i.e. it's started).

Before you create your EC2 instance, set `minecraft.ec2.instance.assign_public_ip` to `yes` and create `vars/namecheap.yml` with your data:

```
# vim: autoindent:expandtab:sw=2:ts=2
---
host: minecraft
domain: your-domain.com
password: your-namecheap-ddns-password
```

and run the playbooks

```
ansible-playbook create.yml
ansible-playbook install.yml
ansible-playbook namecheap.yml
ansible-playbook client.yml
```
