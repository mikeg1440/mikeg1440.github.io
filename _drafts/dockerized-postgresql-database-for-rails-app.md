---
layout: post
title: Dockerized PostgreSQL database for Rails App
---

<div class='text-center'>
  <img src='https://www.google.com/url?sa=i&url=https%3A%2F%2Ftowardsdatascience.com%2Ftricks-for-postgres-and-docker-that-will-make-your-life-easier-fc7bfcba5082&psig=AOvVaw2by2u8zH5KAyotF9RHclXO&ust=1589848550470000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCOjcs8SVvOkCFQAAAAAdAAAAABAD' alt='Docker and PostgreSQL logos' />
</div>

When you develop applications for long you enough your inevitability going to come across a app that requires some version of a product that is different from what you have installed on your system already.  I just came across this where I had an older version of PostgreSQL set up for another app and wanted to setup the new application I was creating with the latest version.  So instead of ending up in this situation again I decided to dockerize the database that I would connect to a Rails application.  This article documents the process I took to do so.

## Create a Volume

Since we want to persist the data in our database we need to create a volume that we can specify in our Dockerfile so that we kill a docker instance and run a new one we our data is persisted. For this example we are going to create a volume called `App_Vol` but you can specify the name of the volume as whatever you want it to be.
```
docker volume create App_Vol
```

## Create the Docker Image

First we need to create a docker image with the correct version and role set up for the application.  Go ahead and create a file with the name `Dockerfile`.  We are going to use the Ubuntu 16.04 base image to build off and add the correct PGP keys to verify the PostgreSQL package so this is the first part of the `Dockerfile`.
```
FROM ubuntu:16.04

RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8
```

Now we need to add the correct repo for the most recent stable release of PostgreSQL.
```
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list
```

There are some dependencies we need to satisfy to correctly run the database so we install these next.
```
RUN apt-get update && apt-get install -y python-software-properties software-properties-common postgresql-9.3 postgresql-client-9.3 postgresql-contrib-9.3
```

Now we need to run the rest of the commands as the user `postgres` to set up the roles and such for the database.  We are going to just the name of the role as `docker` and set the password to `docker` as well but this is just for the walkthrough and you have to make sure to at least change the password to something more secure when doing this in reality.
```
USER postgres

RUN    /etc/init.d/postgresql start &&\
    psql --command "CREATE USER docker WITH SUPERUSER PASSWORD 'docker';" &&\
    createdb -O docker docker
```

We are going to make the database accessible by remote connections, you should note that we do this by setting the ip to `0.0.0.0/0` if you know the specific IP the remote connection is coming from then you should instead specify that to make the database more secure.

```
RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/9.3/main/pg_hba.conf

RUN echo "listen_addresses='*'" >> /etc/postgresql/9.3/main/postgresql.conf
```

We need to expose the port that the database is running on so we connect our app to it.

```
EXPOSE 5432
```

 And to make the data persist we specify our newly created volume `App_Vol`.

```
VOLUME App_Vol
```

Lastly we specify the default command to run the databse when starting the container
```
CMD ["/usr/lib/postgresql/9.3/bin/postgres", "-D", "/var/lib/postgresql/9.3/main", "-c", "config_file=/etc/postgresql/9.3/main/postgresql.conf"]
```

So that completes our Dockerfile, next we are going to build it into a image we can use.

## Build Image

When we build our image we have the chance to set a human readable name, otherwise docker will create a hash for use and assign it to the image. We do this with the `-t` option when we run the build command, for the is example we set the image name to `App_Database`.  While in the same folder as the `Dockerfile` file run this command.
```
docker build . -t App_Database
```

After all the command from our `Dockerfile` are run you should see something like
```
Successfully built 622d93bb55cd
Successfully tagged App_Database:latest
```

And just to verify you can run `docker images` and should see a newly created image with the name `App_Database`.

## Connecting to Rails

Now there are a few things that we need to make sure are configured correctly for the app to connect to the dockerized database.  The first thing is to set the database username and password in the Rails `config/database.yml` file, I would suggest using the Rails encrypted credentials but you can set them however you want.  

Then all we have to do is run our container with a few options specified to allow the app to connect.  First we run docker with the `--rm` flag to remove the container after it stops automatically and `-d` to run it demonized (as a background process), then we set a name for the container to make it easy to manage with the `--name` and finally the `-p 5432:5432` to connect the exposed database port to the localhost port `5432`.

`docker run --rm -d --name postgresql -p 5432:5432  jobhunter_database`

## Checking the Database

If you want to take a look at the database directly after running the migrations or whatnot from the app just make sure that everything looks good you can install the `psql` tool and run this command, obviously change the `-U docker` part to the name of the role you created in the Dockerfile.
```
psql -h localhost -p 5432 -U docker --password
```
