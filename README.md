# Project CheatSheet

### After install VAGRANT

`$ vagrant up` To start the VM
`$ vagrant SSH` Open SSH on the machine

### installing Jenkins

Access Jenkins on selected IP on VagrantFile and port 8080, as Jenkins runs on this port by default. Selected IP can be found on VagrantFile in the section below:

  `# using a specific IP.
  config.vm.network "private_network", ip: "<IP>"`

Note that to get access password, simply run the command with sudo permission on vagrant shell:

`$ cat /var/lib/jenkins/secrets/initialAdminPassword`