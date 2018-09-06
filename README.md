# Domotic Home Assistant

This repository will contain same ideas and information about a domotic configuration with home assitant. It will use a ansible configuration of HomeAssistant running on Ubuntu powered by a postgres database.

Ansible is a powerfull tool that allows you to define recipes and use SSH to run this recipes against some targets, this way all configuration will be done trough code and it allows to replicate them across different devices.

Hass.io is a wonderfull tool but since I spend so much time automating other systems I also wanted to have more control over my HomeAssistant configuration.

## Requirements

* Ansible
* Ubuntu Server

### Vagrant

Vagrant is a awesome that allows you easily create a virtual machine. The project provides a vagrant box configuration that you can use to run the playbook.

## Configuration

To execute the configuration on your system please have a look in the [playbooks/README.md](playbooks/README.md)
