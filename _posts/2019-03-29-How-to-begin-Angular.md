---
title: "Configuring AngularJS and on Linux"
date: 2019-03-14
tags: [linux, frontend, angularjs]
header:
  image: "/images/fort point.png"
excerpt: "Linux, Frontend, AngularJS"
mathjax: "true"
---

# Configuring AngularJS on Ubuntu 18.04

## Getting NodeJS
1. Download the las stable version (here)[https://nodejs.org/en/]
1. Create a new directory on your preference site, for example on your home directory called `apps`
1. Uncompress the file with nautilus or with `tar` command `node-v11.12.0-linux-x64.tar.xz` inside de `apps` directory
1. Create a symbolic to the `nodejs` directory `ln -s node-v11.12.0-linux-x64 node`
1. Append nodejs binaries directory as an environment variable to `~/.bashrc` with `export PATH=$PATH:/home/userhome/apps/node/bin`
1. Refresh your environment `source ~/.bashrc`.

## Install Angular CLI from npm
1. `npm install @angular/cli`
1. `npm i cordova ionic`
1. Now it's time to append module binaries to the environment in `~/.bashrc` with `export PATH=$PATH:/home/userhome/apps/node_modules/.bin`

## Running the local server of your app
1. Go to your application project directory, not the `apps` directory.
1. Compile with `npm i`
1. Run the server `ng serve` and use normally http://localhost:4200.