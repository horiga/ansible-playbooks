Vagrant.configure("2") do |config|

  config.vm.define :my_controller do |my_controller|
    # CentOS 7.6
    my_controller.vm.box = "centos/7"
    my_controller.vm.box_version = "1901.01"
    ENV["LC_ALL"] = "en_US.UTF-8"
    config.ssh.insert_key = false
    my_controller.vm.synced_folder ".", "/home/vagrant/playbook"
    my_controller.vm.network "private_network", ip: "192.168.34.10", :netmask => "255.255.254.0"
    my_controller.vm.provision :hosts, :sync_hosts => true
    my_controller.vm.provision :host_shell do |host_shell|
      host_shell.inline = 'scp -i ~/.vagrant.d/insecure_private_key -o "StrictHostKeyChecking no" ~/.vagrant.d/insecure_private_key vagrant@192.168.34.10:/home/vagrant/.ssh/id_rsa'
    end
    my_controller.vm.provision "shell", inline: <<-SHELL
      chmod 600 /home/vagrant/.ssh/id_rsa
      timedatectl set-timezone Asia/Tokyo
      localectl set-locale LANG=en_US.UTF-8
      yum install -y epel-release
      yum -y update
      yum -y install zip unzip
      yum -y install gcc-c++
      yum -y install python36 python36-libs python36-devel
      python36 -m ensurepip
      /usr/local/bin/pip3 install --upgrade pip
      /usr/local/bin/pip3 install ansible
      SHELL
  end

  N=1
  (1..N).each do |i|
    config.vm.define "target#{i}" do |target|
      # CentOS 7.6
      target.vm.box = "centos/7"
      target.vm.box_version = "1901.01"
      config.ssh.insert_key = false
      target.vm.network "private_network", ip: "192.168.35.#{i}", :netmask => "255.255.254.0"
      target.vm.provision :hosts, :sync_hosts => true
      target.vm.provision "shell", inline: <<-SHELL
        timedatectl set-timezone Asia/Tokyo
        localectl set-locale LANG=en_US.UTF-8
        yum install -y epel-release
        yum -y update
        yum -y install zip unzip
        yum -y install gcc-c++
        yum -y install python36 python36-libs python36-devel
      SHELL
    end
  end

end

