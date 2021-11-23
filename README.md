# Project CheatSheet

### After install VAGRANT

`$ vagrant up` To start the VM
`$ vagrant SSH` Open SSH on the machine

### Installing Jenkins

Access Jenkins browser with selected IP on VagrantFile and port 8080, as Jenkins runs on this port by default. Selected IP can be found on VagrantFile in the section below:

  `# using a specific IP.
  config.vm.network "private_network", ip: "<IP>"`

Note that to get access password, simply run the command with sudo permission on vagrant shell:

`$ cat /var/lib/jenkins/secrets/initialAdminPassword`

### Exposing Docker to external machines

  sudo mkdir -p /etc/systemd/system/docker.service.d/
  sudo vi /etc/systemd/system/docker.service.d/override.conf
      [Service]
      ExecStart=
      ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2376
  sudo systemctl daemon-reload
  sudo systemctl restart docker.service

### Instal plugin Config File Provider

### Configure Managed Files for Dev
    # Name : .env-dev
    [config]
    # Secret configuration
    SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'
    # conf
    DEBUG=True
    # Database
    DB_NAME = "todo_dev"
    DB_USER = "devops_dev"
    DB_PASSWORD = "mestre"
    DB_HOST = "localhost"
    DB_PORT = "3306"

### Configure Managed Files for PRD
    # Name: .env.-prod
    [config]
    # Secret configuration
    SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'
    # conf
    DEBUG=False
    # Database
    DB_NAME = "todo"
    DB_USER = "devops"
    DB_PASSWORD = "mestre"
    DB_HOST = "localhost"
    DB_PORT = "3306"

### At job: jenkins-todo-list-principal import dev env for test:

    Add build step: Provide configuration Files
    File: .env-dev
    Target: ./to_do/.env

    Adicionar passo no build: Executar Shell

#  Script to up the container with env file and test the app:
    #!/bin/sh

    # Up the container
    docker run -d -p 82:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/jenkins-todo-list-principal/to_do/.env:/usr/src/app/to_do/.env --name=todo-list-teste django_todolist_image_build

    # Testing image
    docker exec -i todo-list-teste python manage.py test --keep
    exit_code=$?

    # Removing old container
    docker rm -f todo-list-teste

    if [ $exit_code -ne 0 ]; then
        exit 1
    fi