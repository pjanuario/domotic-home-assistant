# Domotic Home Assistant

This repository will contain same ideas and information about a domotic configuration with home assitant. It will use a ansible configuration of HomeAssistant running on raspberry pi powered by postgres database.

Ansible is a powerfull tool that allows you to define recipes and use SSH to run this recipes against some targets, this way all configuration will be done trough code and it allows to replicate them across different devices.

The raspberry pi will run using Raspbian OS and the rest of the components will run using docker.
Hass.io is a wonderfull tool but since I spend so much time automating other system I also wanted to have more control over my HomeAssistant configuration.

## Requirements

* Ansible
* Raspberry Pi

## Configuration

To execute the configuration on your system please have a look in the [playbooks/README.md](playbooks/README.md)
