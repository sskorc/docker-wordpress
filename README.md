# docker-wordpress

This repository is an example configuration of [Wordpress](http://wordpress.org/), running on multiple
[Docker](http://www.docker.com/) containers. [Docker Compose](http://docs.docker.com/compose/)
is used for orchestration.

## Multiple containers

This environment uses 2 containers:

1. `web` container with Apache2 and PHP, being build from `Dockerfile`. This container shares application sources with
host machine.
2. `db` container with MySQL, pulled directly from [tutum/mysql](https://registry.hub.docker.com/u/tutum/mysql/) image
on Docker Hub. This container shares DB data with boot2docker VM.

Containers are managed by Docker Compose. The configuration is in `docker-compose.yml`.

## Requirements

Basically, you need to have [Docker Compose installed](http://docs.docker.com/compose/#installation-and-set-up).

If you are using Mac OS X or Windows as your host OS, I recommend using [boot2docker](http://boot2docker.io/)
as proxy VM to run Docker.

### Mac OS X
If Mac OS X is your host OS and you use boot2docker to launch Docker you will probably encounter [the bug with writing
to shared volume](https://github.com/boot2docker/boot2docker/issues/581). The following workaround works perfectly:

1. SSH to boot2docker VM: `boot2docker ssh`

2. Edit the `/var/lib/boot2docker/profile` file: `sudo vi /var/lib/boot2docker/profile`

3. Paste the following lines:
    ```
    umount /Users
    mount -t vboxsf -o uid=33,gid=33 Users /Users
    ```

4. Exit the VM and restart it: `boot2docker restart`

## How to use it?

### Start the environment

In the project root directory run the following command:

```
docker-compose up -d
```

This command will build `web` Docker image and run its container together with `db` container.

### Install dependencies

1. Log in to the container by running the following command:
    ```
    docker exec -i -t dockerwordpress_web_1 bash
    ```

2. Install dependencies by running the following command:
    ```
    composer install -n
    ```

### Create database for Wordpress

In project root folder on your host machine run the following command:
```
docker exec -it dockerwordpress_db_1 mysql -u root -e "CREATE DATABASE wordpress;"
```

### Update your hosts

#### Mac OS X

1. Check boot2docker IP address: `boot2docker ip`.

2. Assuming its 192.168.59.103, add the following line to your `/etc/hosts` file:
    ```
    192.168.59.103 wordpress.dev
    ```

#### Linux

TBA

#### Windows

1. Check boot2docker IP address: `boot2docker ip`.

2. Assuming its 192.168.59.103, add the following line to your `%SystemRoot%\System32\drivers\etc\hosts` file:
    ```
    192.168.59.103 wordpress.dev
    ```
