controller_count = 2
controller_prefix = "192.168.55."

node_count = 3

Vagrant.configure("2") do |config|
  config.vbguest.auto_update = false

  (1..controller_count).each do |i|
    config.vm.define "controller#{i}" do |node|
      node.vm.box = "bento/centos-7.6"
      node.vm.host_name = "controller#{i}"
      node.vm.network :private_network, ip: "#{controller_prefix}#{i+1}"
      node.vm.synced_folder ".", "/vagrant", type: "virtualbox"

      node.vm.provider "virtualbox" do |v|
        v.linked_clone = true
        v.customize ["modifyvm", :id, "--memory", 2048]
        v.customize ["modifyvm", :id, "--cpus", 2]
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      end
    end
  end

  if Gem::Platform.local.os == 'linux'
    if ENV["VAGRANT_WSL_ENABLE_WINDOWS_ACCESS"] != ""
      vbox = '/mnt/c/Program\ Files/Oracle/VirtualBox/VBoxManage.exe'
      convert_path = true
    else
      vbox = 'VBoxManage'
      convert_path = false
    end
  else
    vbox = 'C:\Program Files\Oracle\VirtualBox\VBoxManage.exe'
    convert_path = false
  end

  line = `#{vbox} list systemproperties`.lines.select{ |i| i[/\Default machine folder/] }.first
  machine_folder_vbox = line.split(':')[1..-1].join(":").strip()
  if convert_path
    machine_folder = `wslpath -u "#{machine_folder_vbox.gsub("\\", "\\\\")}"`.strip()
  else
    machine_folder = machine_folder_vbox
  end

  (1..node_count).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "bento/centos-7.6"
      node.vm.host_name = "node#{i}"
      node.vm.network :private_network, ip: "#{controller_prefix}#{i+10}"
      node.vm.synced_folder ".", "/vagrant", type: "virtualbox"

      node.vm.provider "virtualbox" do |v|
        v.linked_clone = true
        v.customize ["modifyvm", :id, "--memory", 4096]
        v.customize ["modifyvm", :id, "--cpus", 2]
        v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]

        disk = File.join(machine_folder, "node#{i}", 'disk2.vdi')
        if convert_path
          disk_vbox = `wslpath -w #{disk}`.strip()
        else
          disk_vbox = disk
        end

        unless File.exist?(disk)
          v.customize ['createhd', '--filename', disk_vbox, '--variant', 'Fixed', '--size', 20 * 1024]
        end
        v.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', disk]
      end
    end
  end
end
