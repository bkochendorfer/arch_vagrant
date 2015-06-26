# Helpers
#
def fwd_ports(config, ports)
  ports = [ports] unless ports.respond_to?(:each)

  ports.each do |port|
    config.vm.network :forwarded_port, guest: port, host: port
  end
end

def share_folder(config, source, destiny)
  if File.directory? File.expand_path(source)
    config.vm.synced_folder source, destiny, owner: 'brettk'
  end
end

# Vagrant DSL
#
Vagrant.configure("2") do |config|

  config.vm.box = "scallion-home"
  config.vm.box_url = "http://cloud.terry.im/vagrant/archlinux-x86_64.box"

  config.vm.hostname = "scallion"
  config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["modifyvm", :id, "--cpus", "4"]
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.gui = true
  end

  fwd_ports config, 3000..3010
  fwd_ports config, 3020..3021
  fwd_ports config, 9000..9010

  share_folder config, "~/dev/brighttag", "/mnt/dev/brighttag"

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "./playbook.yml"
  end
end
