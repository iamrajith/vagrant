IMAGE_NAME = "fedora/39-cloud-base"
N = 6

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
    end

    config.vm.define "controlnode" do |control|
        control.vm.box = IMAGE_NAME
        control.vm.network "private_network", ip: "192.168.56.10"
        control.vm.hostname = "controlnode"
        control.vm.provision "shell", inline: <<-SHELL
            #sudo sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
            #sudo sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
            sudo dnf install epel-release -y
            sudo dnf install -y ansible
            sudo useradd rajith -G wheel
            echo -e 'rajith\nrajith' | sudo passwd rajith
        SHELL
    end

    (1..N).each do |i|
        config.vm.define "node#{format('%02d', i)}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.56.#{i + 10}"
            node.vm.hostname = "node#{format('%02d', i)}"
            node.vm.provision "shell", inline: <<-SHELL
                sudo useradd rajith -G wheel
                echo -e 'rajith\nrajith' | sudo passwd rajith
            SHELL
        end
    end
end
