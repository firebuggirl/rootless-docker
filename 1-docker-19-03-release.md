# 19.03 Update and Rootless Docker

https://newsletter.bretfisher.com/issues/devops-and-docker-updates-rootless-docker-201178


- `Rootless Docker` => run Docker w/out root access to the host machine

- ...running as root, any remote application vulnerability can possibly grant someone access to your entire system => `risks greatly increase when  daemon interacts w/ the outside world` => Docker defaults to running as root, but `now can run as a standard user`


## What is Rootless Docker and Why Do I Need It?

- Rootless Docker `allows the root user w/in a container to map itself to a ‘rootless’ user on the host w/ user namespaces` automatically => if Docker daemon is compromised => `attacker will not be able to have total root access on the host`

- creates a `new user namespace` => daemon `starts in that new namespace`, and the `users inside the container are mapped to non-privileged UID` range in the host namespace

## How To Install It?

  - can be installed => similar to Docker w/ an install script:

    https://get.docker.com/rootless?utm_campaign=DevOps%20and%20Docker%20Updates&utm_medium=email&utm_source=Revue%20newsletter


  -  can `run this script (using TLS` to ensure it’s authentic) with:

      ` curl -fsSL https://get.docker.com/rootless -o get-docker.sh | sh `



## Test Rootless Docker In Browser

- Launch containers w/out requiring root:

  https://rev.vu/7PDXJA?utm_campaign=Issue

    - Use Katacode to test this in your browser:

    https://www.katacoda.com/courses/docker/rootless

  - deploy Rootless Docker from a low privileged user => more secure

  - more info: https://engineering.docker.com/2019/02/experimenting-with-rootless-docker/



## Experimenting w/ Rootless Docker from Docker Engineer Tõnis Tiigi

https://medium.com/@tonistiigi/experimenting-with-rootless-docker-416c9ad8c0d6

- Docker engine => often tightly integrated to the features of the Linux kernel

  - EX: 

    - a component of container isolation is based on Linux namespaces => need privileged capabilities to create namespaces & mounting => ie., `root user`

    - still in experimental mode?? 

  - `Getting Rootless Docker` 

    - `install script`:

      ` curl -sSL https://get.docker.com/rootless | sh `

      - run as unprivileged user => will download the latest nightly build of Docker CE, extract it under your home directory and start up the daemon for you

  - `Caveats` => can’t be currently considered a replacement for the full suite of regular Docker engine features

    - things that do not work on rootless mode:

      - cgroups resource controls, apparmor security profiles, checkpoint/restore, overlay networks etc
  
      - Only `Ubuntu based distros support overlay filesystems in rootless mode`

      - NOTE: rootless install script is currently providing nightly builds that `may not be as stable` as you are used to


