# -*- mode: ruby -*-
# vi: set ft=ruby :

IMAGE_NAME = "utm/bookworm"
N = 2

Vagrant.configure("2") do |config|

  config.vm.provider "utm" do |u|
    u.memory = 2048
    u.cpus = 2
  end
      
  config.vm.define "controller" do |controller|
    controller.vm.box = IMAGE_NAME
    
    controller.vm.hostname = "controller"
    # Define a trigger to run before the any provisioner
    controller.trigger.before :provisioner_run, type: :hook do |trigger|
      trigger.info = "Retrieving the IP address of the machine"
      trigger.ruby do |env, machine|
        ip_file = "playbooks/#{machine.name}_ip"
        $ip_address = `utmctl ip-address #{machine.id} | head -n 1 > #{ip_file} && cat #{ip_file}`.lines.first.strip
        puts "The IP address of the machine is: #{$ip_address}"
      end
    end


    controller.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "playbooks/controller-playbook.yml"
      ansible.extra_vars = {
        controller_ip: "{{ lookup('file', 'controller_ip') }}",
      }
    end
  end

  (1..N).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = IMAGE_NAME

      node.vm.hostname = "node#{i}"
      # Define a trigger to run before the any provisioner
      node.trigger.before :provisioner_run, type: :hook do |trigger|
        trigger.info = "Retrieving the IP address of the machine"
        trigger.ruby do |env, machine|
          ip_file = "playbooks/#{machine.name}_ip"
          $ip_address = `utmctl ip-address #{machine.id} | head -n 1 > #{ip_file} && cat #{ip_file}`.lines.first.strip
          puts "The IP address of the machine is: #{$ip_address}"
        end
      end

      node.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/node-playbook.yml"
      end
    end
  end

  # TODO: NOT WORKING , FIX THIS 
  # Add trigger at the end so we have all the IP addresses
  # config.trigger.after :provision do |trigger|
  #   trigger.info = "Updating /etc/hosts on all VMs"
  #   trigger.run = {
  #     inline: <<-SHELL
  #       for vm in controller node1 node2; do
  #         vagrant ssh $vm -c "sudo bash -c 'echo $(cat playbooks/controller_ip) controller >> /etc/hosts'"
  #         for i in $(seq 1 #{N}); do
  #           vagrant ssh $vm -c "sudo bash -c 'echo $(cat playbooks/node${i}_ip) node${i} >> /etc/hosts'"
  #         done
  #       done
  #     SHELL
  #   }
  # end
end