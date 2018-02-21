# -*- mode: ruby -*-
# vi: set ft=ruby :

default_box = "vEOS-lab-4.20.3F-virtualbox"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure("2") do |config|

  # Vagrant vEOS setup architecture:

  # +-------------+           +-------------+
  # |             |  e1   e1  |             |
  # |   spine01   -------------   spine02   |
  # |             |           |             |
  # +------|------\           +------|------+
  #     e2 |       \ e3   e2 /       | e3
  #        |        \       /        |
  #        |         \     /         |
  #        |          \   /          |
  #        |           \ /           |
  #        |            \            |
  #        |           / \           |
  #        |          /   \          |
  #        |         /     \         |
  #        |        /       \        |
  #     e2 |       / e3   e2 \       | e3
  # +------|------+           -------|------+
  # |             |           |             |
  # |   leaf01    -------------   leaf02    |
  # |             |  e1   e1  |             |
  # +-------------+           +-------------+

  # Virtualbox networking note:
  # nic1 is always Management1 which is set to dhcp in the basebox.

  # Create spine01
  config.vm.define "spine01" do |sw|
    sw.vm.box = default_box
    sw.vm.network 'private_network',
                      virtualbox__intnet: 's01s02',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.network 'private_network',
                      virtualbox__intnet: 's01l01',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.network 'private_network',
                      virtualbox__intnet: 's01l02',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.provider :virtualbox do |vb|
      vb.linked_clone = true
      vb.memory = '1536'
    end
    sw.vm.network 'forwarded_port', guest: 443, host: 8441
    sw.vm.provision 'shell', inline: <<-SHELL
      sleep 30
      FastCli -p 15 -c 'configure
      hostname spine01
      interface Management1
        ip address 192.168.56.101/24 secondary
      username vagrant privilege 15 role network-admin secret vagrant
      management api http-commands
        no shutdown
      end'
    SHELL
  end

  # Create spine02
  config.vm.define "spine02" do |sw|
    sw.vm.box = default_box
    sw.vm.network 'private_network',
                      virtualbox__intnet: 's01s02',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.network 'private_network',
                      virtualbox__intnet: 's02l01',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.network 'private_network',
                      virtualbox__intnet: 's02l02',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.provider :virtualbox do |vb|
      vb.linked_clone = true
      vb.memory = '1536'
    end
    sw.vm.network 'forwarded_port', guest: 443, host: 8442
    sw.vm.provision 'shell', inline: <<-SHELL
      sleep 30
      FastCli -p 15 -c 'configure
      hostname spine02
      interface Management1
        ip address 172.19.0.11/24 secondary
      username vagrant privilege 15 role network-admin secret vagrant
      management api http-commands
        no shutdown
      end'
    SHELL
  end

  # Create leaf01
  config.vm.define "leaf01" do |sw|
    sw.vm.box = default_box
    sw.vm.network 'private_network',
                      virtualbox__intnet: 'l01l02',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.network 'private_network',
                      virtualbox__intnet: 's01l01',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.network 'private_network',
                      virtualbox__intnet: 's02l01',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.provider :virtualbox do |vb|
      vb.linked_clone = true
      vb.memory = '1536'
    end
    sw.vm.network 'forwarded_port', guest: 443, host: 8443
    sw.vm.provision 'shell', inline: <<-SHELL
      sleep 30
      FastCli -p 15 -c 'configure
      hostname leaf01
      interface Management1
        ip address 172.19.0.13/24 secondary
      username vagrant privilege 15 role network-admin secret vagrant
      management api http-commands
        no shutdown
      end'
    SHELL
  end

  # Create leaf02
  config.vm.define "leaf02" do |sw|
    sw.vm.box = default_box
    sw.vm.network 'private_network',
                      virtualbox__intnet: 'l01l02',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.network 'private_network',
                      virtualbox__intnet: 's01l02',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.network 'private_network',
                      virtualbox__intnet: 's02l02',
                      ip: '169.254.1.11', auto_config: false
    sw.vm.provider :virtualbox do |vb|
      vb.linked_clone = true
      vb.memory = '1536'
    end
    sw.vm.network 'forwarded_port', guest: 443, host: 8444
    sw.vm.provision 'shell', inline: <<-SHELL
      sleep 30
      FastCli -p 15 -c 'configure
      hostname leaf02
      interface Management1
        ip address 172.19.0.14/24 secondary
      username vagrant privilege 15 role network-admin secret vagrant
      management api http-commands
        no shutdown
      end'
    SHELL
  end
end
