# -*- mode: ruby -*-
# vi: set ft=ruby :

box_name = ENV["CUSTOM_ENV_VAGRANT_BOX_NAME"] || "andreineustroev/ubuntu-ci"
box_version = ENV["CUSTOM_ENV_VAGRANT_BOX_VERSION"] || "0.0.3"
vm_memory = ENV["CUSTOM_ENV_VAGRANT_VM_MEMORY"] || 2048
vm_cpus = ENV["CUSTOM_ENV_VAGRANT_VM_CPUS"] || 2
vm_name = "runner-" + (ENV["CUSTOM_ENV_CI_RUNNER_ID"] || "test").to_s  + \
          "-project-" + (ENV["CUSTOM_ENV_CI_PROJECT_ID"] || "test").to_s  + \
          "-concurrent-" + (ENV["CUSTOM_ENV_CI_CONCURRENT_PROJECT_ID"] || "test").to_s  + \
          "-job-" + (ENV["CUSTOM_ENV_CI_JOB_ID"] || "test").to_s


Vagrant.configure("2") do |config|
  ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'
  config.vm.box = box_name
  if box_version != ""
    config.vm.box_version = box_version
  end
  config.vm.define vm_name = vm_name
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.ssh.username = "gitlab-runner"

  config.vm.provider :libvirt do |libvirt|
    libvirt.uri = 'qemu:///system'
    libvirt.driver = "kvm"
    libvirt.memory = vm_memory
    libvirt.cpus = vm_cpus
    libvirt.storage_pool_name = "default"
    libvirt.disk_driver :cache => 'unsafe'
    libvirt.cpu_mode = "host-passthrough"
    libvirt.disk_bus = "virtio"
  end
end
