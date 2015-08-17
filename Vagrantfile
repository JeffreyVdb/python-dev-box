# shared directories with guest
shared_dirs = {
  # host => guest
  "."               => "/vagrant",
  "salt/roots/"     => "/srv/salt",
  "salt/minions.d"  => "/etc/salt/minion.d"
}

# configure vagrant
Vagrant.configure(2) do |config|
  config.vm.box = "puphpet/ubuntu1404-x64"

  # Share folders
  shared_dirs.each do |host, guest|
    config.vm.synced_folder host, guest, type: "nfs"
  end

  # Salt provisioning
  config.vm.provision :salt do |salt|
    salt.install_master = false         # do not install the master, this is just a single setup
    salt.masterless = true

    salt.minion_config = "salt/minion"
    salt.run_highstate = true
    salt.verbose = true
    salt.bootstrap_options = "-F -c /tmp -P"
  end

  # Additional virtualbox configuration
  ['vmware_fusion', 'vmware_desktop', 'vmware_workstation'].each do |vmware|
    config.vm.provider vmware do |v|
      v.vmx["displayName"] = "pydevbox"
      v.vmx["memsize"] = 1024
      v.vmx["numvcpus"] = 1
      v.vmx["guestOS"] = "ubuntu-64"
    end
  end
end
