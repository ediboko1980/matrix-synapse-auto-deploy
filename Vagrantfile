# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id,
                  "--groups", "/matrix"]
  end
end

Vagrant.configure("2") do |config|
  config.vm.provider :aws do |aws, override|
    aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']
    aws.keypair_name = ENV['AWS_KEYPAIR_NAME']
    aws.instance_type = "m3.medium"
    aws.region = ENV['AWS_REGION']
    aws.subnet_id = ENV['AWS_SUBNET_ID']
    aws.security_groups = ENV['AWS_SECURITY_GROUP_ID']
    aws.ami = ENV['AWS_AMI']
    aws.associate_public_ip = "true"
    aws.tags = {
      'Name' => 'matrix-synapse'
    }
    override.vm.box = "dummy"
    override.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
    override.ssh.username = "ubuntu"
    override.ssh.private_key_path = ENV['SSH_PRIVATE_KEY_PATH']
  end
end

Vagrant.configure('2') do |config|
  config.vm.provider :digital_ocean do |digital_ocean, override|
    override.ssh.private_key_path = ENV['SSH_PRIVATE_KEY_PATH']
    override.vm.box = 'digital_ocean'
    override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"

    digital_ocean.token = ENV['DO_TOKEN']
    digital_ocean.image = 'ubuntu-14-04-x64'
    digital_ocean.region = ENV['DO_REGION']
    digital_ocean.size = '512mb'
  end
end

Vagrant.configure("2") do |config|
  config.vm.define "synapse" do |dev|
    dev.vm.box = "trusty64"
    dev.vm.network :private_network, ip: "10.99.99.230"
    dev.vm.hostname = "matrix-synapse"
    config.ssh.forward_agent = true
    config.vm.provider :virtualbox do |vb|
      vb.name = "matrix-synapse"
      vb.customize ["modifyvm", :id,
                  "--groups", "/matrix",
                  "--memory", "1024"]
    end
  end
end

Vagrant.configure("2") do |config|
  config.vm.provision "ansible" do |ansible|
    ansible.host_key_checking = false
    ansible.sudo = true
    ansible.sudo_user = "vagrant"
    ansible.playbook = "playbook.yml"
#    ansible.raw_arguments = "-vvv"
  end
end
