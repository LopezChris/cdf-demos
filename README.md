# Cloudera Data Flow Demos

## Project Goal

This project was created in order to make demonstrating capabilities of the Cloudera Data Flow Platform more consumable and reproducible. There have been several really awesome demos over the years but they have not been captured in a means that allows for people to spin them up and try for themselves. This project aims to make people more creative; be curious! Get in here and build something fun and then share it with the world!

## Pre-Requisites

At the time of writing Ansible is the only dependencies for using this project. The aim is for all the demos to be built using Ansible and therefore a common consumption pattern. Ansible 2.7 was used at the time of writing. Ansible is a simple and clean Python application written and maintained by RedHat. Installation is particularly easy of OS X and can be accomplished by simply running ```brew install ansible```.

## Running

### Demos

| Demo        | Running           | Description  |
| :-------------: |:-------------:| :-----:|
| weblogs-demo | ansible-playbook -i host weblogs-demo.yml | Demonstrates tailing NGinx info and error logs with MiNiFi C++ agents and streaming those logs to NiFi |

## Deploy CEM

### Instructions

Spin-up an EC2 instance for which you have the private key available, ensure that the appropriate ports are open for NiFi, NiFi Registry and EFM to work properly.

[More Details on ports needed can be found in the CEM documentation](https://docs.hortonworks.com/HDPDocuments/CEM/CEM-1.0.0/installation/content/install-efm-server.html)

Next, clone this repository onto your local computer

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

Once Ansible is done configuring your AWS machine you may navigate to EFM and NiFi

### EFM

```text
<AWS-public-host-name>:10080/efm/ui/
```

### NiFi

```text
<AWS-public-host-name>:8080/nifi/
```

## Small MiNiFi C++ Agent

SSH on to the machine assigned to be the agent

~~~bash
wget http://mirrors.ibiblio.org/apache/nifi/nifi-minifi-cpp/0.6.0/nifi-minifi-cpp-bionic-0.6.0-bin.tar.gz

tar -xvf nifi-minifi-cpp-bionic-0.6.0-bin.tar.gz

scp -i ~/.ssh/Cfarnes-CEM.pem ~/Desktop/minifi.properties ubuntu@ec2-54-215-240-129.us-west-1.compute.amazonaws.com:/home/ubuntu/nifi-minifi-cpp-0.6.0/conf

vi minifi.properties
~~~

Enter your public host name in these fields

```nifi.c2.agent.coap.host=<Public DNS>```

```nifi.c2.flow.base.url=<Public DNS>:10080/efm/api```

```nifi.c2.rest.url=<Public DNS>:10080/efm/api/c2-protocol/heartbeat```

```nifi.c2.rest.url.ack=<Public DNS>:10080/efm/api/c2-protocol/acknowledge```