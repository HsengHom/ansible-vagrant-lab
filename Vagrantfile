# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'etc'

# Change this to the absolute path of your SSH public key
# (e.g. '/home/user123/.ssh/id_rsa.pub')
PUBLIC_KEY_PATH = '/Users/hsenghom/pov-vagrant/.ssh/nhha_ssh.pub'.freeze

Vagrant.configure('2') do |config|
  config.vm.box = 'bento/ubuntu-24.04'

  # Prevent mounting of local directory into vm with `, disabled: true`
  config.vm.synced_folder '.', '/home/vagrant', disabled: true

  config.vm.provision 'shell' do |shell|
    shell.inline = <<~DOC
      # Allow us to SSH using our public key rather than using `vagrant ssh`
      echo '#{IO.read(PUBLIC_KEY_PATH)}' >> /home/vagrant/.ssh/authorized_keys
    DOC
  end

  config.vm.provider 'vmware_desktop' do |v|
    # Use the bare minimum amount of RAM
    v.vmx['memsize'] = '768'

    # Set CPUs to half the cores detected by Ruby, which should work best in
    # most cases
    v.vmx['numvcpus'] = (Etc.nprocessors / 2.0).ceil
  end

  # Define the VMs
  config.vm.define 'ansible' do |m|
    m.vm.hostname = 'ansible'
    m.vm.network "private_network", ip: "192.168.56.99" # vmnet2 private host-only network
  end
end