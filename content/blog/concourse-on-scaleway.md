+++
title = "The world's cheapest concourse server"
date = "2017-03-17T17:13:40Z"
tags = "concourse"
description = "How to deploy concourse on scaleway"
draft = true
+++

## Steps

1. Spin up scaleway from docker image
  - cant deploy concourse with bosh, as there is no scaleway cpi yet
  - we could use the binary, but concourse doesnt publish ARM binaries (and I'm too lazy too build it)
1. Download docker-compose yml from concourse website
  - wget https://goo.gl/yJidcM

# Concourse

On [The World's Smallest Concourse Server](http://engineering.pivotal.io/post/worlds-smallest-concourse-server/), 

# Scaleway costs

# Lets encrypt

# Deploying without bosh


wget https://github.com/concourse/concourse/releases/download/v2.7.0/concourse_linux_amd64
apt-get install postgresql
ssh-keygen -t rsa -f tsa_host_key -N ''
ssh-keygen -t rsa -f worker_key -N ''
ssh-keygen -t rsa -f session_signing_key -N ''
cp worker_key.pub authorized_worker_keys

