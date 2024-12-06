# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "gusztavvargadr/windows-11"
  config.vm.provider "hyperv" do |h|
    h.vmname = "win11"
    h.enable_virtualization_extensions = true 
  end

  config.trigger.before :"VagrantPlugins::HyperV::Action::WaitForIPAddress", type: :action do |t|
    t.info = 'Sleep to prevent issues where Windows hasn’t completed Applying computer settings'
    t.run = { inline: "Start-Sleep -Seconds 180" }
    end
  config.vm.communicator = "winrm"
  config.winrm.username = "vagrant"
  config.winrm.password = "vagrant"
  config.vm.boot_timeout = 0

  # workaround for gusztavvargadr/packer#420
  config.winrm.transport = :plaintext
  config.winrm.basic_auth_only = true

  # Increase WinRM timeout settings
  config.winrm.timeout = 18000
  config.winrm.retry_limit = 10
  # Add a delay to ensure the VM is fully booted
  #config.vm.provision "shell", inline: "sleep 240", run: "always"

  # provisioning
  config.vm.provision "shell", path: "scripts/keyboard.ps1", privileged: false
  config.vm.provision "shell", path: "scripts/network.ps1", privileged: false
  config.vm.provision "shell", path: "scripts/debloat.ps1", privileged: false
  #config.vm.provision "shell", path: "scripts/defender_config.ps1", privileged: false
  #config.vm.provision "shell", path: "scripts/software.ps1", privileged: false
  config.vm.provision "shell", path: "scripts/update.ps1", privileged: true
  config.vm.synced_folder ".", "/vagrant", type: "smb", smb_username: "vagrant", smb_password: "vagrant"
  config.vm.provision "shell", reboot: true

end
