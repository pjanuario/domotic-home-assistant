# Ansible playbooks

This directory contains ansible playbooks to configure several components of the domotic system.

## Requirements

This playbook was only tested on Ubuntu Bionic.

### Install raspbian

To install rasbian follow the steps mentioned in the [raspberry pi website](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)

### Enable SSH on boot

By default raspberry pi know come with ssh disabled, to enable it just create a empty file on the boot directory with the name `ssh`.

### Enable Wifi on boot

To configure the wifi on boot, just add the `wpa_supplicant.conf` to the boot directory. You can use the template on the pi-wifi role as guideline present at [roles/pi-wifi/templates/wpa_supplicant.conf](roles/pi-wifi/templates/wpa_supplicant.conf)

## Dependencies

### SSHPASS

When we enabled ssh on the boot of the raspberry pi by default it will use the user `pi` and the passowrd: `raspberry`.
There by to access it with ansible we need to install sshpass in your compute to run the playbooks.

Instructions: https://gist.github.com/arunoda/7790979

## Configure Inventory

The inventory contains the instructions on how to access the different servers and the groups they belong. Just copy the file `home.inv.example` to `home.inv` and update the ip of your hosts.

## Connect to the host

Once you configured `home.inv` executing the following command allow you to test the inventory and ping all hosts.

    $ ansible -m ping all -u pi -e 'ansible_ssh_pass=raspberry'

By this time you should be able to receive pong from your host and see `green` lines.

## Ansible facts cache

Facts are information about the host that ansible uses to execute the playbooks. In the the current configuration present on `ansible.cfg` we cache the facts to make playbook execution faster.

You can now execute the following command allow retrieve the facts from all the hosts.

    $ ansible -m setup all -u pi -e 'ansible_ssh_pass=raspberry'

By this time you should be able to receive facts from your host and see `green` lines.

*NOTE: This step isnt required since ansible is smart enough to gather them on a playbook execution.*

### Cleanup the facts cache

To cleanup the cache you can either remove the folder `.ansible_facts` or run the previous command once again. Keep in mind if you already run the playbooks to include your ssh user and perform the hardening you should not pass `-u pi -e 'ansible_ssh_pass=raspberry'`

## Adjust the configuration

### Store the vault master secret

The file `.vault-domotic.example` contains a example of a vault master secret, copy the file to `.vault-domotic` and store your master secret.

NOTE: The `.vault-domotic` is added to `.gitignore` file to prevent that it ends on the source control.

### Create the secrets

For each secret property that exists in `group_vars/all.yaml` you need to execute the following command:

    $ ansible-vault encrypt_string 'changeme' --name 'pi_password'

Pick the output and replace on the file.

### Ansible Galaxy Roles

Install ansible galaxy roles.

    $ ansible-galaxy install -r requirements.yml

## Create a admin user

The bootstrap role is going to remove the default user, so before we run it we need to ensure that the we can connect using ssh with a new admin user.
Please make sure you have ssh configurations and your ssh config file defines a default user that is the one present in the configuration.

    $ ansible-playbook playbook.yaml -u pi -e 'ansible_ssh_pass=raspberry' --tags bootstrap-user

Once the playbook run you should be able to run the following command

    $ ansible -m ping all

**NOTE:** If the following command isnt working please do not proceed to next steps. Most likely you are missing the user define on the ssh config or you can also pass using `-u`.

## Run the playbook

    $ ansible-playbook playbook.yaml
