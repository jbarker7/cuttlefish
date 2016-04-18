# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "dummy"

  config.vm.network "forwarded_port", guest: 443, host: 8443

  config.vm.provider :aws do |aws, override|
      aws.access_key_id = ENV["ACCESS_KEY_ID"]
      aws.secret_access_key = ENV["SECRET_ACCESS_KEY"]
      aws.region = "us-east-1"

      aws.keypair_name = "vagrant"
      aws.instance_type= "t2.small"
      aws.security_groups = [ 'vagrant' ]

      aws.elastic_ip = ENV["IP_ADDRESS"]

      override.ssh.username = "ubuntu" #defaults: http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html
      override.ssh.private_key_path = "vagrant.pem"

      # More comprehensive region config
      aws.region_config "us-east-1" do |region|
          region.ami = "ami-d05e75b8" #amazon linux not supported - "ami-e3106686"
      end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.vault_password_file = "~/.cuttlefish_ansible_vault_pass.txt"
  end
end
