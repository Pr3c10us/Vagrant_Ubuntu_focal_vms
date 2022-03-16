Vagrant.configure("2") do |config|
    servers=[
        {
          :hostname => "ServerBoss",
          :box => "ubuntu/focal64",
          :ip => "172.16.1.50",
          :ip2 => "192.168.0.1",
          :ssh_port => '2200'
        },
        {
          :hostname => "Server2",
          :box => "ubuntu/focal64",
          :ip => "172.16.1.51",
          :ip2 => "192.168.0.2",
          :ssh_port => '2201'
        },
        {
          :hostname => "Server3",
          :box => "ubuntu/focal64",
          :ip => "172.16.1.52",
          :ip2 => "192.168.0.3",
          :ssh_port => '2202'
        }
      ]

    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.box = machine[:box]
            node.vm.hostname = machine[:hostname]
            node.vm.network :private_network, ip: machine[:ip]
            node.vm.network "public_network", ip: machine[:ip2]

            node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"
            # node.vm.synced_folder "../data", "/home/vagrant/data"
            node.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"
            node.vm.provider :virtualbox do |vb|
                vb.customize ["modifyvm", :id, "--memory", 512]
                vb.customize ["modifyvm", :id, "--cpus", 1]
                vb.customize ["modifyvm", :id, "--nat-network3", "precious"]
            end
        end
    end
end