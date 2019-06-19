Peakaboo
========

## A real time dashboard for the remote monitoring of Opencast compatible capture agents

![Alt text](docs/peakaboo.png?raw=true "Peakaboo-roomlist")

### To run in development

Installing the extra node packages:
```shell
meteor npm install
```

```shell
mkdir config
cp docs/settings.example.json config/settings.json
chmod u+x run
./run
```

The meteor applcation will build then point a web browser at:
```shell
http://localhost:3000
```

### To run in in production
Peakaboo has been dockerized so its super straight forward to get this running in a production environment. There are however alternatives to running meteor apps in production but they aren't covered here.

Its highly recomended you run this as a non-root user. Create a new user, a good name would be `peakaboo`

First generate a valid SSL certificate. You can generate a self signed certificate or you could also use Letsencrypt with certbot https://letsencrypt.org/.

You will also need to create a new `.htpasswd` file to allow clients to POST images back to peakaboo

move into the docker folder and place your SSL key file and certificate into the Nginx
```shell
cd .docker/
cp /path/to/ssl-key.file nginx/my.key
cp /path/to/ssl-cert.file nginx/my.crt
cp /path/to/.htpasswd nginx/.htpasswd
```

Install `docker` and `docker-compose`. Please refer to the latest docker documenation on how to get started on your platform https://docs.docker.com/engine/installation/
https://docs.docker.com/compose/
you can also install the latest `docker-compose` via pip
```shell
pip install docker-compose
```

In production Meteor apps get their settings from enviroment variables, these can be set in the `.bash_profile` or exported on the fly. Export your settings JSON and give set the `SERVER_NAME` to match your DNS hostname of the server.
```shell
export METEOR_SETTINGS=$(cat ~/path/to/settings.json)
export SERVER_NAME=peakaboo.example.com
```

To Build the peakaboo docker container run:

```shell
docker-compose build peakaboo
```
CentOS 6:
To divert docker's logs to syslog so that they are auto rotated and don't fill up the
disk, make sure `/etc/sysconfig/docker` has:

```shell
other_args="--log-driver=syslog"
```

Then run peakaboo!
```
docker-compose up -d
```

When there's a change peakaboo you can update the docker images by running `docker-compose up` with the `--build` flag. This will rebuild the peakaboo docker container and start the newly created container.
```
docker-compose up --build -d
```

Client Side
-----------

To send/recieve data from the peakaboo app you will need to use a client that uses the Distributed data protocol from Meteor. You could write your own, or check out the examples in the docs directory: [Peakaboo Plugins](docs/capture_agent_plugins/examples)
