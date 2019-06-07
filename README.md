# Cloudera Data Flow Demos

## Project Goal

This project was created in order to make demonstrating capabilties of the Cloudera Data Flow Platform more consumable and reproducable. There have been several really awesome demos over the years but they have not been captured in a means that allows for people to spin them up and try for themselves. This project aims to make people more creative; be curious! Get in here and build something fun and then share it with the world!

## Pre-Requisites

At the time of writing Ansible is the only dependencies for using this project. The aim is for all the demos to be built using Ansible and therefore a common consumption pattern. Ansible 2.7 was used at the time of writing. Ansible is a simple and clean Python application written and maintained by RedHat. Installation is particularly easy of OS X and can be accomplished by simply running ```brew install ansible```.

## Running

## Demos

| Demo        | Running           | Description  |
| :-------------: |:-------------:| :-----:|
| weblogs-demo | ansible-playbook -i host weblogs-demo.yml | Demonstrates tailing NGinx info and error logs with MiNiFi C++ agents and streaming those logs to NiFi |

## Instructions

Spin-up a few EC2 instances for which you have the private key available to you, ensure that the appropriate ports are open for NiFi, NiFi Registry and EFM to work properly.

Clone this repository

~~~bash
git clone https://github.com/jdye64/cdf-demos.git
~~~

### Edit the host file

~~~bash
cd cdf-demos/ansible && vi host
~~~

Get the public DNS of your instaces on the EC2 console and place it in the host file.

Fill in the `ansible_user` with the user login information given in the EC2 console (e.g. `centos` for a CentOS7 instance)

Finally, give the full path your `.pem` private key to the `ansible_ssh_private_key_file` field in the host file and run the command below

~~~bash
ansible-playbook -i host efm-10-install.yml
~~~

### EFM

```text
<AWS-public-host-name>:10080/efm/ui/
```

### NiFi

```text
<AWS-public-host-name>:8080/nifi/
```
