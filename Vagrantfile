# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  config.vm.box = "Debian-7.1.0-amd64-netboot-ugent-v0.1"

  config.vm.box_url = "http://users.ugent.be/~tberton/baseboxes/Debian-7.1.0-amd64-netboot-ugent-v0.1.box"

  config.vm.define "server" do |server|
    server.vm.network :hostonly, "192.168.33.40"
  end

  config.vm.define "client1" do |client1|
    client1.vm.network :hostonly, "192.168.33.41"
  end

  config.vm.define "client2" do |client2|
    client2.vm.network :hostonly, "192.168.33.42"
  end
end
