# Run Rootless via Vagrant => w/ @Bret Fisher & @spiddy

https://github.com/spiddy/docker-rootless

- Rootless will not run on Mac => need a Linux kernel

  https://www.youtube.com/watch?v=tEH-Kk5EMZ0

  11:21

  - create Vagrantfile => create 2 machines =>

      - `rootless` (Docker in rootless mode)

      - `rootful` (normal installation of Docker)

  - RUN:

## Create environment => Run:

       ` vagrant up `

       - make sure both machines are running:

        ` vagrant status `

## Run Rootless in one terminal:

    ` vagrant ssh rootless `

    ` cd /vagrant `

    ` mkdir scripts `

    ` cd scripts `

    ` touch setup.sh `

    ` sudo vim setup.sh `//create script to prepare the OS for Docker rootless mode

          - How do I run .sh files in Linux?

          https://www.cyberciti.biz/faq/run-execute-sh-shell-script/

          - Set execute permission on your script:

             ` chmod +x script-name-here.sh `

             ie.,... ` chmod +x setup.sh `

          - To run your script, enter:

              ` ./script-name-here.sh `

              ` ./setup.sh`

              - OR

              ` sh script-name-here.sh `

              ` sh setup.sh `

              - OR

              ` bash script-name-here.sh `

              ` bash setup.sh `


    ` cd ~ `

    ` curl -sSL https://get.docker.com/rootless | sh `//creates a service in systemd => Docker binaries are installed in /home/vagrant/bin

          - NOTE: normally NOT a good idea to sh scripts from the web, w/ this case being the exception because of known origin...


    - socket resides w/in this user space:

        `unix:///run/user/1000/docker.sock`

    - Run:

     ` export DOCKER_HOST=unix:///run/user/1000/docker.sock `

    - now can run Docker commands:

      ` docker ps `

       ` docker ps -a `

      ` ps aux | grep docker ` //can see that there is no root user running anything..



## Run Rootful in another terminal:

  ` vagrant ssh rootful `

  ` curl -fsSL https://get.docker.com -o get-docker.sh `

  ` chmod +x get-docker.sh `

  ` ./get-docker.sh `

  ` docker ps `

  ` sudo docker ps `

  ` ps aux | grep docker `//can see that Docker is running as root



...continue @ 16:00...
