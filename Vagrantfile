# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

#  config.vm.box = "oraclelinux-6-x86_64"
#  config.vm.box_url = "http://cloud.terry.im/vagrant/oraclelinux-6-x86_64.box"
  config.vm.box = "bento/centos-6.7"
  config.vm.hostname = "oracle"

  config.ssh.forward_x11=true

  # Forward Oracle ports
  config.vm.network :forwarded_port, guest: 1521, host: 1521
  config.vm.network :forwarded_port, guest: 1158, host: 1158

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id,
                  "--name", "oradb",
                  "--memory", "2048",
                  "--natdnshostresolver1", "on"]
  end

  config.vm.provision :shell, :inline => "sudo rm /etc/localtime && sudo ln -s /usr/share/zoneinfo/US/Eastern /etc/localtime"

  ###==> add oracle-yum repo
  config.vm.provision :shell, :inline => "cd /etc/yum.repos.d && wget http://public-yum.oracle.com/public-yum-ol6.repo && sudo rpm --import http://oss.oracle.com/ol6/RPM-GPG-KEY-oracle && sudo rpm -q gpg-pubkey-ec551f03-4c2d256a || echo \"maybe already provisioned?\""

  ###==> add puppet repo
  ###==> http://java.paykin.info/install-oracle-database-vagrant-puppet/
  config.vm.provision :shell, :inline => "sudo rpm -ivh http://yum.puppetlabs.com/puppetlabs-release-el-6.noarch.rpm || echo \"maybe already provisioned?\""

  config.vm.provision :shell, :inline => "sudo yum install puppet -y"

  config.vbguest.auto_update = false

  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "manifests"
    puppet.module_path = "modules"
    puppet.manifest_file = "base.pp"
    puppet.options = "--verbose --trace"
  end
end
