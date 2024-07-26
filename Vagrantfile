# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :inetRouter => {
        :box_name => "generic/centos7",
        :vm_name => "inetRouter",
        :net => [
                   {adapter: 2, auto_config: false, virtualbox__intnet: "router-net"},
                   {adapter: 3, auto_config: false, virtualbox__intnet: "router-net"},
                   {ip: '192.168.56.10', adapter: 8},
                ]
  },
  :centralRouter => {
        :box_name => "generic/centos7",
        :vm_name => "centralRouter",
        :net => [
                   {adapter: 2, auto_config: false, virtualbox__intnet: "router-net"},
                   {adapter: 3, auto_config: false, virtualbox__intnet: "router-net"},
                   {ip: '192.168.255.9', adapter: 6, netmask: "255.255.255.252", virtualbox__intnet: "office1-central"},
                   {ip: '192.168.56.11', adapter: 8},
                ]
  },

  :office1Router => {
        :box_name => "generic/centos7",
        :vm_name => "office1Router",
        :net => [
                   {ip: '192.168.255.10', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "office1-central"},
                   {adapter: 3, auto_config: false, virtualbox__intnet: "vlan1"},
                   {adapter: 4, auto_config: false, virtualbox__intnet: "vlan1"},
                   {adapter: 5, auto_config: false, virtualbox__intnet: "vlan2"},
                   {adapter: 6, auto_config: false, virtualbox__intnet: "vlan2"},
                   {ip: '192.168.56.20', adapter: 8},
                ]
  },

  :testClient1 => {
        :box_name => "generic/centos7",
        :vm_name => "testClient1",
        :net => [
                   {adapter: 2, auto_config: false, virtualbox__intnet: "testLAN"},
                   {ip: '192.168.56.21', adapter: 8},
                ]
  },

  :testServer1 => {
        :box_name => "generic/centos7",
        :vm_name => "testServer1",
        :net => [
                   {adapter: 2, auto_config: false, virtualbox__intnet: "testLAN"},
                   {ip: '192.168.56.22', adapter: 8},
            ]
  },

  :testClient2 => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "testClient2",
        :net => [
                   {adapter: 2, auto_config: false, virtualbox__intnet: "testLAN"},
                   {ip: '192.168.56.31', adapter: 8},
                ]
  },

  :testServer2 => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "testServer2",
        :net => [
                   {adapter: 2, auto_config: false, virtualbox__intnet: "testLAN"},
                   {ip: '192.168.56.32', adapter: 8},
                ]
  },

}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
    
    config.vm.define boxname do |box|
   
      box.vm.box = boxconfig[:box_name]
      box.vm.hostname = boxconfig[:vm_name]

      box.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 2
       end

      #  boxconfig[:net].each do |net_conf|
      #   box.vm.network "private_network", ip: net_conf[:ip], adapter: net_conf[:adapter], netmask: net_conf[:netmask], virtualbox__intnet: net_conf[:virtualbox__intnet], auto_config: net_conf[:auto_config]
      # end

      boxconfig[:net].each do |net_conf|
        if net_conf[:ip]
          # Если IP указан, то конфигурируем с ним
          box.vm.network "private_network", ip: net_conf[:ip], adapter: net_conf[:adapter], netmask: net_conf[:netmask], virtualbox__intnet: net_conf[:virtualbox__intnet], auto_config: net_conf.fetch(:auto_config, true)
        else
          # Если IP не указан, но указан intnet и adapter, то конфигурируем без IP
          box.vm.network "private_network", adapter: net_conf[:adapter], virtualbox__intnet: net_conf[:virtualbox__intnet], auto_config: net_conf[:auto_config]
        end
      end

      ssh_pub_key = File.readlines("../id_rsa.pub").first.strip
      box.vm.provision "shell", inline: <<-SHELL
        echo #{ssh_pub_key} >> ~vagrant/.ssh/authorized_keys
        echo #{ssh_pub_key} >> ~root/.ssh/authorized_keys
        sudo sed -i 's/\#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
        systemctl restart sshd
      SHELL
    end
  end
end
