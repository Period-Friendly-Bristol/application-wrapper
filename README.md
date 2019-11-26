# Period Dignity Application Wrapper

This is a simple wrapper to host setup and Docker compose file for all the containers for the Period Dignity Application

## Installation
run `./setup.sh` in root folder

If you encounter `command not found` error, run `chmod +x setup.sh`

## Running the whole stack
run `docker-compose up server` and go to `localhost:3000` to access the Frontend

## Running Django Server

run `docker-compose up server` 

## Running Commands in Server

run `docker-compose run --rm server bash`

