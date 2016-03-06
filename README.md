# simple-sinatra-app-server
Provision a new application server and deploy the Sinatra application

## Prerequisite for Deployment
* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](https://www.vagrantup.com/)
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Ansible](http://http://docs.ansible.com/ansible/intro_installation.html)


######  Step 1. Download the Vagrant file and Ansible Playbooks from the git repo.

    mkdir ~/vagrant
    cd ~/vagrant
    git clone https://github.com/kai-test/simple-sinatra-app-server



######  Step 2. Spin up a VM (Ubuntu 14.04). 

    cd ~/vagrant/simple-sinatra-app-server
    vagrant up

Note: I forward the port 80 of guest VM to the port 3080 of host.


######  Step 3. After VM is up and running, use Ansible to 1) Create system users 2) Install base packages, Ruby, Unicorn, Nginx. 3) Do security configuration (firewall blocks all incoming traffics except SSH, HTTP and HTTPS, root direct login is blocked, password authentication is disabled and only key authentication is allowed).

    cd ~/vagrant/simple-sinatra-app-server
    export ANSIBLE_HOST_KEY_CHECKING=False
    ansible-playbook -i playbooks/inventory playbooks/server_provision.yml --tags "install"


######  Step 4. After Ansible run is complete, you should be able to do the test:   
* On Host machine, use web browser to check the ip (0.0.0.0:3080). The webpage should show 'Hello World!'.
* Inside Guest VM, 'curl 0.0.0.0:80 'to get the 'Hello World!' as output.


## Note:
###### In real-world, except Vagrant VM, any type of machines such as bare-metal, AWS EC2 instance, Openstack Nova instance can be deployed using this Ansible receipt. To do that, simply add destination machhine address to the inventory file: 


    ~/vagrant/simple-sinatra-app-server/playbooks/inventory/hosts


###### Also, the Ansible playbooks can be used to start unicorn and nginx services:


    ansible-playbook -i playbooks/inventory playbooks/server_provision.yml --tags "start_service"


###### To stop unicorn and nginx services:


    ansible-playbook -i playbooks/inventory playbooks/server_provision.yml --tags "stop_service"


###### To restart unicorn and nginx services:


    ansible-playbook -i playbooks/inventory playbooks/server_provision.yml --tags "restart_service"

