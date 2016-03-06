VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "sinatra-app-server" do |server|
    server.vm.box = "ubuntu/trusty64"
    server.ssh.forward_agent = true
    server.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 1
    end
    server.vm.network "forwarded_port", guest: 80, host: 3080
    server.vm.network "private_network", ip: "192.168.50.5"
  end
end

