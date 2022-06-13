Vagrant.configure(2) do |config|
    config.vm.provider "virtualbox" do |v|
        v.memory = 4096
        v.cpus = 4
    end
    config.vm.box = "centos/7"
    end

    config.vm.define "server" do |server|
        server.vm.hostname = "server"
        server.vm.network "private_network", ip: "10.2.3.4"
    end

    config.vm.define "runner" do |runner|
        runner.vm.hostname = "runner"
        runner.vm.network "private_network", ip: "10.2.3.5"
    end
