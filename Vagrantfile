# -*- mode: ruby -*-
# vi: set ft=ruby :

box_url = "http://tiny.si/pequod_base.box"

ARGV.each do |com|
  if com.length > 1 && com[0,2] == '--'
    if com.index('=')
      opt = com[0,com.index('=')]
      arg = com[com.index('=')+1,com.length]
      case opt
      when '--box_url'
        puts("Setting url to #{arg}")
        box_url = arg
      end
    end
  end
end

Vagrant.configure("2") do |config|
  
  config.vm.box_url = box_url 
  # default to using tiny.si url
  config.vm.box = "ProfessionallyEvil/pequod" 
  
  # config.vm.box = "pequod_base.box"
  # config.vm.box_url = "http://tiny.si/pequod_base.box"
  config.vm.define "pequod", primary: true do |pequod|
    # Every Vagrant development environment requires a box. You can search for
    # boxes at https://vagrantcloud.com/search.
    # config.vm.synced_folder
    config.ssh.username = "ahab"
    config.vm.provider :virtualbox do |vb|
      vb.gui = false
      vb.name = "pequod"
    end
  
    config.vm.provision "shell", inline: "echo Call me Ishmael."
    config.vm.provision "shell", inline: "mkdir -p /home/ahab/.scripts"
    config.vm.provision "shell", inline: "cp -r /vagrant/provisions/* /home/ahab/.scripts"
    config.vm.provision "shell", path: "provisions/install/docker.sh"
    config.vm.provision "shell", path: "provisions/targets/start_docker_targets.sh"
    config.vm.provision "shell", inline: <<-SHELL
    (crontab -l 2>/dev/null; echo "@reboot root /home/ahab/.scripts/provisions/targets/start_docker_targets.sh") | crontab -
    SHELL
    config.vm.provision "shell", inline: "echo Thus, I give up the spear!"
  end
end
