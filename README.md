# simple-sinatra-app-server
Provision a new application server and deploy the Sinatra application

## Prerequisite for Deployment (Need to be installed)
* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](https://www.vagrantup.com/)
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Ansible](http://docs.ansible.com/ansible/intro_installation.html#installing-the-control-machine)

## Machine Provision, Server Configuration and Test 
######  Step 1. Download the Vagrant file and Ansible Playbooks from the git repo.

    mkdir ~/vagrant
    cd ~/vagrant
    git clone https://github.com/kai-test/simple-sinatra-app-server



######  Step 2. Spin up a VM (Ubuntu 14.04). 

    cd ~/vagrant/simple-sinatra-app-server
    vagrant up

Note: I forward the port 80 of guest VM to the port 3080 of host.


######  Step 3. After VM is up and running, use Ansible to 1) Create two system users - 'unicorn' and 'nginx'  2) Install base system packages, Ruby, Unicorn, Nginx. 3) Configure security rules: a) Enable firewall to block all incoming traffics except SSH, HTTP and HTTPS. b) Disable 'root' direct login. c) Disable password authentication and only allow key authentication.

    cd ~/vagrant/simple-sinatra-app-server
    export ANSIBLE_HOST_KEY_CHECKING=False
    ansible-playbook -i playbooks/inventory playbooks/server_provision.yml --tags "install"


######  Step 4. After Ansible run is complete, you should be able to do the test:   
* On Host machine, use web browser to check the ip (0.0.0.0:3080 or 192.168.50.5).  The webpage should show 'Hello World!'.
* Go inside the Guest VM (via 'vagrant ssh'), try 'curl 0.0.0.0:80', should  get the 'Hello World!' as output.


## Note:
###### In real-world, except Vagrant VM, other types of machine such as bare-metal, AWS EC2 instance, Openstack Nova instance can be deployed using this Ansible receipt. To do that, simply add destination machine's IP address to the inventory file: 


    ~/vagrant/simple-sinatra-app-server/playbooks/inventory/hosts


###### Also, the Ansible playbooks can be used to start unicorn and nginx services:


    ansible-playbook -i playbooks/inventory playbooks/server_provision.yml --tags "start_service"


###### To stop unicorn and nginx services:


    ansible-playbook -i playbooks/inventory playbooks/server_provision.yml --tags "stop_service"


###### To restart unicorn and nginx services:


    ansible-playbook -i playbooks/inventory playbooks/server_provision.yml --tags "restart_service"

