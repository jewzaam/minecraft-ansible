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

## Create Server

```
ansible-playbook create.yml
```

Will create an inventory file `.inventory` for use with server setup.  If this file is deleted it will be recreated.

## Setup Minecraft

TODO

## Teardown Server

Only tears down the instance & EIP at this time.

```
ansible-playbook teardown.yml
```

# Gotyas

If you need to recreate your server you'll probably hit a problem of reusing the same `id` for the ec2 instance.  To get around this, edit `var/minecraft.yml` and change the value of `ec2.instance.id` to something that hasn't been used in the last 24 hours.
