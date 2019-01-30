# howtographql.com with docker

This repository is a result of my process of using docker to complete the
[howtographql.com][howtographql] without installing node, prisma, prisma-cli,
or anything other than docker and git locally.

Instead, I use docker containers with the prisma-cli installed, and
mount them to my local working directory to execute prisma commands.

My process was generally thus:

1. [Build a `prisma` image](#build-prisma-docker-image)
1. [Write wrapper scripts for `npm` and `prisma`](#creating-wrapper-scripts)
1. [Use `docker-compose` to run the app](#using-docker-compose)

I've created tags for what your `docker-compose` and project should look like
at the completion of each of the chapters from [howtographql.com][howtographql].
You can view those tags on the [releases][] page of this repo.

## Build `prisma` docker image

First, I built an image from `node:latest` and basically ran `npm i -g prisma` on it.
You can either copy my [Dockerfile][prisma-dockerfile], and run `docker build -t prisma .`,
or you can use my published image by pulling `docker pull longlivechief/prisma`.

Building yourself will guarantee the latest prisma-cli, and will be shorter to type if you skip
the [wrapper scripts](#creating-wrapper-scripts) step and opt to run the full docker commands by hand.

## Creating wrapper scripts

This step is optional, but will save you a lot of typing. I use docker to execute
`prisma` and `npm` commands. (howtographql still uses `yarn` but you can substitute
`yarn` for `npm` in the steps below if you want).

The wrapper script will also give you the "native" feel and allow you to follow along
with the tutorials as if `prisma` and `npm` _were_ natively installed, so I highly recommend
creating these wrappers/aliases. If you choose not to, simply run the `docker run` commmand
from each script by hand each time you want to run `prisma <command>` or `npm <command`.

Copy or clone the two scripts in `/bin` of this project somewhere onto your `$PATH`. For this
example let's assume you put them on `$HOME/bin`.
Ensure they are executable by running `chmod +x $HOME/bin/npm $HOME/bin/prisma`

## Using `docker-compose`

Use `node:latest` or a derivative image to run the code from each chapter.
Use `docker-compose up -d` to start up app components, and `docker-compose restart` to reload app
components after a change. Map ports `4000:4000` in order to access the playground app on `localhost:4000`
as instructed during the demo.

Mount your working directory to `/usr/app`, and ensure `/usr/app` is your working directory.

[howtographql]: https://howtographql.com
[prisma-dockerfile]: https://github.com/LongLiveCHIEF/prisma-cli-docker/blob/master/Dockerfile
[releases]: https://github.com/LongLiveCHIEF/docker-howtographql/releases
