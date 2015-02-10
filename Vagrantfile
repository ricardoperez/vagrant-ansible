VAGRANTFILE_API_VERSION = "2"
Vagrant.require_version ">= 1.3.5"

def define_vm(vm_config, vm_name, vm_namespace, vm_ip, vm_dns_patterns)
  vm_config.vm.define vm_name do |box|
    box.vm.network :private_network, ip: vm_ip, netmask: '255.255.255.0'
    box.vm.hostname = "#{vm_name}-#{vm_namespace}"
    box.dns.patterns = vm_dns_patterns

    box.vm.provision :ansible do |ansible|
      ansible.playbook = 'provision.yml'
    end

    box.vm.box = "ubuntu/trusty64"
    box.vm.provider :virtualbox do |vb|
      vb.gui = false
      vb.customize [ 'modifyvm', :id, '--natdnshostresolver1', 'on' ]
    end
  end
end

Vagrant.configure(VAGRANTFILE_API_VERSION = "2") do |config|
  config.vm.boot_timeout = 60

  config.ssh.forward_agent = true

  config.dns.tld = 'vagrant'

  define_vm config, 'test', 'ansible', '10.0.0.120', [/app-test.dev.vagrant$/]
end
