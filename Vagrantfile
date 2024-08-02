Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/jammy64"
    config.vm.box_version = "20240801.0.0"

    config.vm.define "elk" do |elk|
        elk.vm.hostname = "elk"
        elk.vm.network :private_network, ip: "192.168.50.10"
        elk.vm.provider "virtualbox" do |vb|
          vb.name = "elk"
          vb.memory = 12288
          vb.cpus = 4
        end
    end
    
  end