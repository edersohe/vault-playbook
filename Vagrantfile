# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.provider 'virtualbox' do |vb|
    vb.memory = '2048'
  end

  nodes = [
    'u1',
    'u2',
    'u3',
    'p1',
    'p2',
    'p3',
  ]

  nodes.each_with_index { |name, index|
    config.vm.define name do |node|
      node.vm.box = 'almalinux/8'
      node.vm.hostname = name
      node.vm.network :forwarded_port, guest: 22, host: 2200 + index, id: 'ssh'
      node.vm.network :public_network, bridge: "enp4s0", ip: "192.168.1.#{110 + index}"
    end
  }

end


