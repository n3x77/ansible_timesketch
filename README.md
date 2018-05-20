# Ansible

Ansible is a simple automation language that can perfectly describe an IT application infrastructure. It’s easy-to-learn, self-documenting, and doesn’t require a grad-level computer science degree to read. Automation shouldn’t be more complex than the tasks it’s replacing.

This Ansible Playbook allows the Installation of Timesketch from the current HEAD and builds everything that is needed for productive usage like:
* Installation of Yara 3.7.1 (If you want to use Plaso with Yara)
* Installation of currently supported Plaso Version 20180127
* Installation of Elasticsearch 5 (including Java 8)
* Installation and Building of yarn and nodejs_8 for the Web-UI-Components of Timesketch
* Installation of neo4j
* Installation of Timesketch from current HEAD
* Installation of GUNICORN, NGINX as WSGI-Server with SSL-support (self-signed Cert)


## Perparation

The following steps need to be done before the Ansible playbook can be deployed on the Timesketch box

1. Install Ansible for your operating system
2. Create your Timesketch box
3. Install minimal Python environment on your Timesketch box and setup SSH-Access

### Install Ansible

This role has been developed and testet with Ansible version 2.4. Due to the fact that there were a lot of changes in the lastest update,
this playbook needs to be updated.

Follow the official instructions [here](http://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

### Create your Timesketch box
You can use this Playbook on a hardware or virtual machine on various Hypervisors. The only thing that is needed for ansible
is an passwordless SSH-Connection to the machine with an privileged user. (Sudo must be enabled)

This playbook has been developed on Ubuntu Server 16.04 LTS, but migration should be easy for e.g. Debian Stretch.

### Install minimal Python environment
Ansible needs a minimal Python environment on the Timesketch box.

On Ubuntu you can install Python in Version (2.7) using apt:

`sudo apt install python-minimal`

### Setup SSH-Access to Timesketch box

*Note*: Due to the fact that ansible has currently some problems with password based ssh connections just create an user that has sudo rights on the Timeksetch box and genereate an SSH-Keypair that can connect to the box add the following settings
to your local ~/.ssh/config file:

```Host timesketch-box
    User                <USER>
    Hostname            <IP-Timeketch-BOX>
    IdentityFile        ~/.ssh/<YOUR-SSH-KEY>
```

To add your ssh key to the authorized key file on the timesketch box you can use `ssh-copy-id`. For adding the key just type:
`ssh-copy-id -i <YOUR-SSH-KEY> <USER>@<IP-Timesketch-BOX>`

To test the setup just enter `ssh timesketch-box`. You should be able to connect to the timesketch box without user and password.

## Execution of the Playbook

Befor running the Ansible Playbook you need to define the following variables in the file `group_vars/all`:
```
ansible_user: "timesketch"
ansible_ssh_user: "timesketch"
ansible_ssh_pass: "timesktch"
```

* `ansible_user`: is the local user on the timesketch box that has the rights to do sudo. This user is used for the installation on the box.
* `ansible_ssh_user`: is the user that is used by the ssh connection
* `ansible_ssh_pass`: Is the password for the ssh connection


### Ansible Hosts file:
To specifiy on which servers the Playbook should be run you have to add them to the `hosts`-file. You can add one server per line.

*Note*: If you use the name that you have specified in your local ssh config (see section: Setup SSH-Access to Timesketch box) this settings will be used for the connection.

```[timesketch-servers]
timesketch-box
```

### Run Playbook

To run the Playbook and start the deployment of Timesketch run the following command:
`ansible-playbook -i host -K timesketch.yml`

For debugging purpose you can add the verbose -v flag:
`ansible-playbook -i host -K timesketch.yml -v`
`ansible-playbook -i host -K timesketch.yml -vvv`

## Authors

* **stuhli** - *Ansible enhancments* - [stuhli](https://github.com/stuhli)
* **n3x77** - *Initial work* - [n3x77](https://github.com/n3x77)

## License

This project is licensed under the BSD 3-Clause License - see the [LICENSE.md](LICENSE.md) file for details
