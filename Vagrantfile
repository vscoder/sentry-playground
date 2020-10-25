# Describe VMs
MACHINES = {
  # VM name "kernel update"
  :"sentry-server" => {
              # VM box
              :box_name => "centos/7",
              # VM CPU count
              :cpus => 4,
              # VM RAM size (Mb)
              :memory => 4096,
              # networks
              :net => [
                { :ip => "192.168.10.10" },
              ],
              # forwarded ports
              :forwarded_port => [
                # sentry
                { :guest => 9000, :host => 19000 },
              ],
              # provision files
              :provision_files => [],
              # provision scripts
              :provision_scripts => [
                { :path => "./provision/0-whoami.sh" }
              ],
            },
  # :"hw16-web" => {
  #             # VM box
  #             :box_name => "centos/7",
  #             # VM CPU count
  #             :cpus => 2,
  #             # VM RAM size (Mb)
  #             :memory => 512,
  #             # networks
  #             :net => [
  #               { :ip => "192.168.10.11" },
  #             ],
  #             # forwarded ports
  #             :forwarded_port => [
  #               # web server
  #               { :guest => 80, :host => 1080 },
  #             ],
  #             # provision files
  #             :provision_files => [],
  #             # provision scripts
  #             :provision_scripts => [
  #               { :path => "./provision/0-whoami.sh" }
  #             ],
  #           }
}

ANSIBLE_GROUPS = {
  # Central log server
  "server" => [ "sentry-server" ],
  # Web server
  #"web" => [ "client" ],
}

# # Function to check whether VM was already provisioned
# def provisioned?(vm_name='default', provider='virtualbox')
#   File.exist?(".vagrant/machines/#{vm_name}/#{provider}/action_provision")
# end

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    # Disable shared folders if not provisioned
    config.vm.synced_folder ".", "/vagrant", type: "virtualbox", disabled: true
    # Apply VM config
    config.vm.define boxname do |box|
      # Set VM base box and hostname
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxname.to_s
      # Additional network config if present
      if boxconfig.key?(:net)
        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end
      end
      # Port-forward config if present
      if boxconfig.key?(:forwarded_port)
        boxconfig[:forwarded_port].each do |port|
          box.vm.network "forwarded_port", port
        end
      end
      # VM resources config
      box.vm.provider "virtualbox" do |v|
        # Set VM RAM size and CPU count
        v.memory = boxconfig[:memory]
        v.cpus = boxconfig[:cpus]
      end
      # provision files
      if boxconfig.key?(:provision_files)
        boxconfig[:provision_files].each do |file|
          box.vm.provision "file",
              # source file path on host
              source: file[:source],
              # destination file path on VM
              destination: file[:destination]
        end
      end
      # provision scripts
      if boxconfig.key?(:provision_scripts)
        boxconfig[:provision_scripts].each do |script|
          box.vm.provision "shell",
              # Path to script
              path: script[:path],
              # Script's args
              args: script[:args],
              # Set environment variables for script
              env: {
                "PROVISIONER" => "vagrant",
                "VAGRANT_HOST" => boxname,
              }
          ## reload VM
          # config.vagrant.plugins = ["vagrant-reload"]
          # box.vm.provision :reload if script[:reload]
        end
      end
      # Yes, that's a lot of magic to provision all instances
      # after the last one becomes up
      # https://www.vagrantup.com/docs/provisioning/ansible.html#ansible-parallel-execution
      if boxname.to_s == "sentry-server"
        box.vm.provision "ansible" do |ansible|
          ansible.limit = "all"
          ansible.playbook = "provision.yml"
          ansible.groups = ANSIBLE_GROUPS
        end
      end
    end
  end
end
