nodes = [ 
  { :hostname => 'iscsi-ansible', :ip => '10.250.33.10', :box => 'centos8', :ansible => 'yes' },
  { :hostname => 'iscsi-srv', :ip => '10.250.33.100', :box => 'centos8' }, 
  { :hostname => 'iscci-cln01',  :ip => '10.250.33.101', :box => 'centos8', :ram => 1024}, 
  { :hostname => 'iscsi-cln02',  :ip => '10.250.33.102', :box => 'centos8', :ram => 1024}, 
  { :hostname => 'iscsi-cln03',  :ip => '10.250.33.103', :box => 'centos8', :ram => 1024},
] 
 
Vagrant.configure("2") do |config| 
  nodes.each do |node| 
    config.vm.define node[:hostname] do |nodeconfig| 
      nodeconfig.vm.box = "bento/centos-8" 
	  nodeconfig.vm.box_version = "202006.16.0"
      nodeconfig.vm.hostname = node[:hostname] 
      nodeconfig.vm.network :private_network, ip: node[:ip] 
	  
      memory = node[:ram] ? node[:ram] : 512; 
      nodeconfig.vm.provider :virtualbox do |vb| 
        vb.customize [ 
          "modifyvm", :id, "--memory", memory.to_s, "--natdnshostresolver1", "on"
        ]
		if node[:ansible] == "yes"
			nodeconfig.vm.provision "shell",
				inline: "dnf install git epel-release -y"
			nodeconfig.vm.provision "shell",
				inline: "dnf install ansible -y"
		end
      end 
    end
    config.hostmanager.enabled = true 
    config.hostmanager.manage_guest = true 
  end 
end 