# Ansible playbooks

This directory contains ansible playbooks to configure several components of the domotic system.


## Dependencies

### SSHPASS

When we enabled ssh on the boot of the raspberry pi by default it will use the user `pi` and the passowrd: `raspberry`.
There by to access it with ansible we need to install sshpass.

Instructions: https://gist.github.com/arunoda/7790979

# Configure Inventory

The inventory contains the instructions on how to access the different servers and the groups they belong. Just copy the file `home.inv.example` to `home.inv` and update the ip of your hosts.

# Connect to the host

Once you configured `home.inv` executing the following command allow you to test the inventory and ping all hosts.

    $ ansible -m ping all -u pi -e 'ansible_ssh_pass=raspberry'

By this time you should be able to receive pong from your host and see `green` lines.

# Ansible facts cache

Facts are information about the host that ansible uses to execute the playbooks. In the the current configuration present on `ansible.cfg` we cache the facts to make playbook execution faster.

You can now execute the following command allow retrieve the facts from all the hosts.

    $ ansible -m setup all -u pi -e 'ansible_ssh_pass=raspberry'

By this time you should be able to receive facts from your host and see `green` lines.

*NOTE: This step isnt required since ansible is smart enough to gather them on a playbook execution.*

## Cleanup the facts cache

To cleanup the cache you can either remove the folder `.ansible_facts` or run the previous command once again. Keep in mind if you already run the playbooks to include your ssh user and perform the hardening you should not pass `-u pi -e 'ansible_ssh_pass=raspberry'`

# Adjust the configuration

## Store the vault master secret

The file `.vault-domotic.example` contains a example of a vault master secret, copy the file to `.vault-domotic` and store your master secret.

NOTE: The `.vault-domotic` is added to `.gitignore` file to prevent that it ends on the source control.

## Create the secrets

For each secret property that exists in `group_vars/all.yaml` you need to execute the following command:

    $ ansible-vault encrypt_string --vault-id .vault-domotic 'changeme' --name 'pi_password'

Pick the output and replace on the file.
