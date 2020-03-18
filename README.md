# Period Dignity Application Wrapper

This is a simple wrapper to host setup and Docker compose file for all the containers for the Period Dignity Application.

Ansible is used to provision the production infrastructure. You can use Vagrant to create a VM to test this provisioning in.

## Installation

run `./setup.sh` in root folder

If you encounter `command not found` error, run `chmod +x setup.sh`

## Running the whole stack

run `docker-compose up`
go to `localhost:3000` to access the site

### Troubleshooting common errors

#### `ERR_CONNECTION_REFUSED` on localhost:3000

React-scripts are not running

run `cd client`
run `npm install`
run `npm start`

visit `localhost:3000`

#### `FATAL: password authentication failed for user "friendly"`

You need to clean out old pfb containers which include the database.

_Warning these commands will delete data from other docker containers so make sure everything you need is backed up or you know what you are doing._

run `docker-compose stop && docker-compose rm && docker volume prune`

run `docker ps -a` to check there are no pfb containers remaining.

run `docker-compose up` again.

#### `Error starting userland proxy: listen tcp 0.0.0.0:80: bind: address already in use`

You need to stop Apache.

Try `sudo apachectl stop`

## Running Django Server

run `docker-compose up server`

## Running Commands in Server

run `docker-compose run --rm server bash`

## Provisioning

To provision the applications in a local staging environment, follow these steps:

1. Download Virtualbox to spin up VMs by following [these instructions](https://www.virtualbox.org/wiki/Downloads)
2. Download Vagrant to manage these VMs by following [these instructions](https://www.vagrantup.com/downloads.html)
3. Download Ansible to provision these VMs by following [these instructions](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#latest-releases-via-pip) - scroll down a bit to install inside a Python virtual environment
4. Ensure the IP addresses defined in `Vagrantfile` and `ansible/inventory.cfg` are the same and are free on your network
5. Open a linux shell & `cd` to this `period-dignity-app-wrapper` directory
6. Start the VM using the Vagrant command `vagrant up` (this starts a VM with the configuration defined in `Vagrantfile`)
7. `cd` to the `ansible` directory
8. Acquire the Ansible Vault password from the system administrator, and enter this into a file called `vault_password`
9. Follow the remaining instructions in `ansible/README.md#Deployment`

## Info

The `.rsync-filter` file specifies which files to exclude when synchronising files between the Ansible Host and the target VM.

Environment variables specified in `.env` are loaded by `docker-compose`. This file provides sensible defaults, however, most of these are overridden by Ansible. For example, in `ansible/roles/period_poverty/tasks/main.yml`, environment variables are overridden with Ansible variables (spefified in `defaults/main.yml` or `vars/main.yml`).
