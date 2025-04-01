MACHINES = {
  :master1 => {
        :box_name => "bento/ubuntu-22.04",
        #:box_version => "202407.23.0",
        :ip_addr => "192.168.255.80",
        #:base_mac => "255.255.255.252"
  },

  :worker1 => {
        :box_name => "bento/ubuntu-22.04",
        #:box_version => "202407.23.0",
        :ip_addr => "192.168.255.81",
        #:base_mac => "255.255.255.0"
  },

  :worker2 => {
        :box_name => "bento/ubuntu-22.04",
        #:box_version => "202407.23.0",
        :ip_addr => '192.168.255.82'
  }

}


Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.ssh.username = 'vagrant'
    config.ssh.password = 'vagrant'
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.box_version = boxconfig[:box_version]
      box.vm.host_name = boxname.to_s
      #box.vm.network "private_network", ip: boxconfig[:ip_addr], base_mac: boxconfig[:base_mac], nat_device: boxconfig[:nat_device]
      box.vm.network "private_network", ip: boxconfig[:ip_addr], nat_device: boxconfig[:nat_device]
      config.vm.provider :vmware_desktop do |vmware|
        vmware.gui = true
        vmware.memory = 4048
        vmware.cpus = 2
        #vmware.vmx["ethernet0.virtualdev"] = "vmxnet3"
        vmware.ssh_info_public = true
        vmware.linked_clone = false
      end

      box.vm.provision "shell", inline: <<-SHELL
      mkdir -p ~root/.ssh
      cp ~vagrant/.ssh/auth* ~root/.ssh
      sudo sed -i 's/\#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config
      sudo sed -i 's/\#PubkeyAuthentication yes/PubkeyAuthentication yes/g' /etc/ssh/sshd_config
      sudo sed -i 's/\#Port 22/#Port 22/g' /etc/ssh/sshd_config
      systemctl restart sshd
    	  SHELL
          end
        end

#      config.vm.provision "ansible" do |ansible|
#      ansible.playbook = "./project.yaml"
#      ansible.inventory_path = "staging/hosts"
#       end
      end
