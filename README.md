# Slurm on Mac using UTM

Vagrant setup for a Slurm cluster on Mac using UTM.

> This project sets up your own HPC cluster using Slurm on Mac with UTM VM cluster.

## Prerequisites
* [Vagrant](https://www.vagrantup.com/)
* [UTM](https://mac.getutm.app/)
* [Vagrant UTM plugin](https://naveenrajm7.github.io/vagrant_utm/)
* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

## How to use

1. Clone the repository
```bash
git clone https://github.com/naveenrajm7/slurm-utm.git
```

2. Execute Vagrant inside the folder

```bash
cd slurm-utm
vagrant up
```

3. Access the controller and use Slurm

```bash
# Accessing the controller
$ vagrant ssh controller
...
vagrant@controller:~$ sinfo
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug*       up   infinite      2   idle node[1-2]
```

## Credits

* [slurm-for-dummies](https://github.com/SergioMEV/slurm-for-dummies)