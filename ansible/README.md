# Period Dignity Application Wrapper

This is a simple wrapper to host setup and Docker compose file for all the containers for the Period Dignity Application

## Installation

This project can be deployed using Ansible. The main deployment script is `site.yml`, which refers to community and local roles.

To deploy the site, use the following command:
`ansible-playbook -i inventory.cfg --vault-password-file .vault-password site.yml`

This requires a file called .vault-password to exist which contains the password to decrypt variables in `roles/period_poverty/vars/main.yml`.

This command can be used to show the encrypted variables on the command line:
`ansible localhost -m debug -a var="django_password" -e "@roles/period_poverty/vars/main.yml" --vault-password-file .vault-password`

run `./setup.sh` in root folder

If you encounter `command not found` error, run `chmod +x setup.sh`

## Running Django Server

run `docker-compose up server`

## Running Commands in Server

run `docker-compose run --rm server bash`
