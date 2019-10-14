# Katacode Rootless Docker 

https://www.katacoda.com/courses/docker/rootless


- Rootless Docker => a project from Docker => removes the requirement for the Docker Daemon to be started by a root user => creates a more secure environment


- deploy Rootless Docker from a low privileged user, and how user will be able to manage and control the containers running on the system

### More information at:

  https://engineering.docker.com/2019/02/experimenting-with-rootless-docker/


## Start:

### Step 1 - Create Ubuntu User

  - environment is currently running Ubuntu 16.04 with the user logged in as root 

  - `create a new user` w/out these root privileges => create new user named `lowprivuser` w/ the default permissions:

    ` useradd -m -d /home/lowprivuser -p $(openssl passwd -1 password) lowprivuser `

    - NOTE: create different user w/ different name that is not pushed to Github => save in password manager &/or variables.env....

  - switch to running as this new, low privileged user:

    ` sudo su lowprivuser `

    - user is `not able to create or change files in certain locations such as the root directory`

    - also not able to access Docker as previously

        - EX: ` docker ps ` fails..denied !!


## Step 2 - Install Rootless Docker

  - deploy the required components for the new Rootless version

  - `Run the following command as lowprivuser` to execute the script and install the components:

      ` curl -sSL https://get.docker.com/rootless | sh `

## Step 3 - Access Docker

  - Rootless Docker has now been installed. The `daemon can be started` using the following script:

    ` export XDG_RUNTIME_DIR=/tmp/docker-1001
      export PATH=/home/lowprivuser/bin:$PATH
      export DOCKER_HOST=unix:///tmp/docker-1001/docker.sock

      /home/lowprivuser/bin/dockerd-rootless.sh --
      experimental --storage-driver vfs `

      - runs in the foreground => allows you to see debug output from Rootless Docker Daemon

  - `launch a second terminal window` and change the context to `run as roootlessuser`:

    ` sudo su lowprivuser `

  - access Docker, set the following env vars => connecting to the Docker instance running for the `user w/ id 1001`, which `should match the id of lowprivuser`

    ` export XDG_RUNTIME_DIR=/tmp/docker-1001
      export PATH=/home/lowprivuser/bin:$PATH
      export DOCKER_HOST=unix:///tmp/docker-1001/docker.sock `


## Step 4 - Run Containers

  - access the Docker Daemon running for user 1001

    ` docker ps `

  - inspect details of the Daemon:

    ` docker info `

  - enter bash container:

    ` docker run -it ubuntu bash `

  - Users within the container will `still be reported as root` => able to install packages and modify parts of the system running inside Docker. However, if they managed to break out they `wouldn't be able to interfere with the host system`

    ` id ` 
    
    - returns :

      - uid=0(root) gid=0(root) groups=0(root)

  - In separate terminal window as root => `explore which processes are running and which user started them`
  
    - `verify` that new container instance is `managed + owned by low privileged user`:

      ` id; ps aux | grep lowprivuser `


### now running Docker Containers w/out requiring any additional permissions === better security 
