# Practical section

## API Service
This repository contains `Rates API` which provide average rates between two ports. The API fetches data from a postgres 
database. For development purpose sample database schema and a dataset can be found in the `db` directory.

## Running the API locally
To ease the development and testing of the service, we have added a `docker compose` instruction file which boostrap a
postgres database with some dummy data and run the API. You must install `Docker Desktop` for Mac/Windows or both `Docker` 
runtime and `docker-compose` tool for any other platform.
Installation instructions can be found in [Docker installation guide](https://docs.docker.com/engine/install/) and
[dokcer-compose installation guide](https://docs.docker.com/compose/install/)

API locates the database configuration via environment variables and the `.env` file contains default values for each variable.
You may change the `.env` file as you fit but it not required to do so.

**NOTE: DB\_HOST environment variable value MUST match with the `docker compose` database service configuration.**

All `docker-compose` commands must be run from the project root directory.

### Build
```
docker-compose build
```

### Run
```
docker-compose up # Runs the containers in forground, you can see the container logs in the same command window
docker-compose up -d # Runs the container in the background
```

### Some useful docker-compose commands
```
docker-compose logs # view all service logs
docker-compose logs <compose service name> # view logs related to given service
docker-compose exec <compose service name> /bin/bash # log into running contianer for debug purpose
docker-compose stop # Stops all containers running as part of the docker compose configuration
```

## Testing the application locally
By default, the API is runs on [http://localhost:3000](http://localhost:3000). You can verify the functionality by using
a request as below,
```
curl "http://127.0.0.1:3000/rates?date_from=2016-01-01&date_to=2016-01-31&orig_code=CNGGZ&dest_code=EETLL"
```

The output should be something like this:
```
{
   "rates" : [
      {
         "count" : 3,
         "day" : "2016-01-31",
         "price" : 1154.33333333333
      },
      {
         "count" : 3,
         "day" : "2016-01-30",
         "price" : 1154.33333333333
      },
      ...
   ]
}
```

## Debugging container issues
### Rates API container exits abnormally during startup
Rates API startup depends on the postgres database. If the database is not accepting connections when the API container starts
it will exit with a python exception. If you encounter this issue, try increasing the number of `retries`
in the database service health check configuration in the `docker-compose.yaml` file.

```
healthcheck:
  test: psql -h localhost -U postgres -c "SELECT 'alive'"
  interval: 10s
  retries: 3 # increase this
``` 

## Why docker compose
Docker is the most user-friendly, and a feature rich container runtime currently available. Docker compose is the
container orchestration tool for Docker. By defining a desired state of one or more containers using a human readable declarative syntax
we can orchestrate complex environments very easily, and the same container environments can be duplicated in any
OS/platform without worrying about any underlying OS differences and dependencies( in most cases :) ). Developers no need to worry about
how to set up environments, applications, network configuration etc. because it all taken care of by containers and container orchestration.
