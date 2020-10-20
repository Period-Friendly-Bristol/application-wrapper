# Period Dignity Application Wrapper

This repository joins the client and the server applications using submodules and docker-compose.

Additionally, it holds the Ansible deployment scripts that are used to provision these applications on a server.

## Setup

To set this project up for running locally, or in preparation for deployment, first clone the repository and then run `./setup.sh` to initialise the submodules.

## Running locally

To run a local developement version of the site, simply run `docker compose up`.

If you make changes to an application, e.g. `server` or `client`, you will need to re-build the application before running `docker-compose up` to see the changes.

To re-build a specific application, run `docker-compose build {server,client}`. The name `server` or `client` come from the `docker-compose.yml` file.

To re-build all applications (which is the simplest way of ensuring changes show up when developing), run `docker-compose up --build`.

## Deploying

To deploy the applications on Bristol Is Open's cloud infrastructure, you must be connected to their network using a VPN.

Additionally, you must have Ansible installed on your local machine. Follow these steps to get set up:

### Ansible setup

```bash
# Move to the ansible directory
cd period-dignity-app-wrapper/ansible

# Create the Ansible virtual environment
python3 -m venv ~/ansible-env

# Activate the ansible environment
source ~/ansible-env/bin/activate

# Install Ansible
pip install ansible

# Install the third-party Ansible roles used in the playbook
ansible-galaxy install -r ansible-requirements.yml
```

### Ansible deployment

Deploy to the production server, follow the these steps:

```bash
# Move to the ansible directory
cd period-dignity-app-wrapper/ansible

# Activate the ansible environment
source ~/ansible-env/bin/activate

# Deploy the applications
ansible-playbook -i environments/prod site.yml
```

## Extra info

The `.rsync-filter` file specifies which files to exclude when synchronising files between the Ansible Host and the target VM.

Environment variables specified in `.env` are loaded by `docker-compose` and are passed to containers using the `args` and `environment` options in the `docker-compose.yml` file. The `.env` file provides sensible defaults, however, most of these are overridden by Ansible. For example, in `ansible/roles/period_poverty/tasks/main.yml`, we can see that we override environment variables using Ansible variables. 

Ansible variables are spefified in `ansible/roles/period_poverty/defaults/main.yml`, `ansible/roles/period_poverty/vars/main.yml`, and `ansible/environments/prod/group_vars/all.yml`. The order of precedence in this list is least to highest - [more information can be found here](https://docs.ansible.com/ansible/2.5/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable).
