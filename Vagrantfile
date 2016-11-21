# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 tw=0 et :

role = File.basename(File.expand_path(File.dirname(__FILE__)))

ENV['ANSIBLE_ROLES_PATH'] = "../"

boxes = [
  {
    :name => "ubuntu-1404",
    :box => "ubuntu/trusty64",
    :ip => '10.0.77.12',
    :cpu => "33",
    :ram => "256"
  },
]

Vagrant.configure("2") do |config|
  boxes.each do |box|
    config.vm.define box[:name] do |vms|
      vms.vm.box = box[:box]
      vms.vm.box_url = box[:url]
      vms.vm.hostname = "ansible-#{role}-#{box[:name]}"

      vms.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--cpuexecutioncap", box[:cpu]]
        v.customize ["modifyvm", :id, "--memory", box[:ram]]
      end

      vms.vm.network :private_network, ip: box[:ip]

      vms.vm.provision :ansible do |ansible|
        ansible.playbook = "tests/vagrant.yml"
        ansible.galaxy_role_file = "requirements.yml"
        ansible.verbose = "vv"
      end
    end
  end
end
