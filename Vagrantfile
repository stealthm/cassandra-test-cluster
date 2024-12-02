Vagrant.configure("2") do |config|

  config.vm.define "server" do |config|
    config.vm.box = "gusztavvargadr/ubuntu-server-2204-lts"
    config.vm.box_check_update = false
    config.vm.hostname = "cassandra-server"
    config.vm.network "public_network", ip: "192.168.1.197", hostname: true
#    config.vm.network "public_network", ip: "192.168.1.197", hostname: true, bridge: "en0: Ethernet"
    config.vm.provider "virtualbox" do |vb|
      vb.name = "cassandra-server"
      vb.gui = false
      vb.cpus = 2
      vb.memory = "3072"
    end
    config.vm.provision "shell" do |s|
    ssh_pub_key = if File.exist?("#{Dir.home}/.ssh/id_rsa.pub")
                File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
              else
                ""
              end
     s.inline = <<-SHELL
     echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
     echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
     apt-get update -y
     apt-get install -y docker-compose
    SHELL
    end
    config.vm.provision "file", source: "docker-compose.yml", destination: "~/docker-compose.yml"
  end

  config.vm.define "client" do |config|
    config.vm.box = "gusztavvargadr/ubuntu-server-2204-lts"
    config.vm.box_check_update = false
    config.vm.hostname = "cassandra-client"
    config.vm.network "public_network", ip: "192.168.1.198", hostname: true
#    config.vm.network "public_network", ip: "192.168.1.198", hostname: true, bridge: "en0: Ethernet"
    config.vm.provider "virtualbox" do |vb|
      vb.name = "cassandra-client"
      vb.gui = false
      vb.cpus = 2
      vb.memory = "1024"
    end
    config.vm.provision "shell" do |s|
    ssh_pub_key = if File.exist?("#{Dir.home}/.ssh/id_rsa.pub")
                File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
              else
                ""
              end
     s.inline = <<-SHELL
     echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
     echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
     apt-get update -y
     snap install cqlsh
    SHELL
    end
  end

end

