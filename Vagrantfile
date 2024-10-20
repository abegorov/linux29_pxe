# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'

Vagrant.configure("2") do |config|
  config.vm.define :'pxe-server' do |host|
    host.vm.box = 'almalinux/9/v9.4.20240805'
    host.vm.host_name = 'pxe-server.internal'
    host.vm.provider :libvirt do |libvirt|
      libvirt.cpus = 2
      libvirt.memory = 4096
      libvirt.storage :file, size: '50G', type: 'qcow2',
        bus: 'virtio', device: 'vdb'
    end

    host.vm.network :private_network, ip: '192.168.50.20',
      libvirt__network_name: 'pxenet', libvirt__dhcp_enabled: false,
      libvirt__forward_mode: 'veryisolated'

    host.vm.provision :ansible do |ansible|
      ansible.playbook = 'playbook.yml'
      ansible.compatibility_mode = '2.0'
      ansible.raw_arguments = ['--diff']
    end
  end

  config.vm.define :'pxe-client', autostart: false do |host|
    host.vm.host_name = 'pxe-client.internal'
    host.vm.provider :libvirt do |libvirt|
      libvirt.cpus = 2
      libvirt.memory = 12144
      libvirt.storage :file, size: '50G', type: 'qcow2',
        bus: 'virtio', device: 'vda'

      libvirt.boot 'hd'
      libvirt.boot 'network' => 'pxenet'

      # необходимо для загрузки по сети (без этого не работает):
      libvirt.loader = '/usr/share/qemu/ovmf-x86_64.bin'
      libvirt.random :model => 'random'
    end

    host.vm.network :private_network, auto_config: false,
      libvirt__network_name: 'pxenet', libvirt__dhcp_enabled: false,
      libvirt__forward_mode: 'veryisolated'
  end
end
