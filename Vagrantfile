# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "windows2012r2"

  host_port = 5895
  config.winrm.host = "localhost"
  config.winrm.port = host_port
  config.winrm.guest_port = host_port
  config.vm.guest = :windows
  config.vm.communicator = "winrm"
  config.vm.network :forwarded_port, guest: 3389, host: 3399,      id: "rdp",   auto_correct: false
  config.vm.network :forwarded_port, guest: 5985, host: host_port, id: "winrm", auto_correct: false

  # Build + Test Application
  #    88   config.vm.provision "shell", path: "vagrant/shell/build.ps1", run: "always"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.customize ["modifyvm", :id, "--memory",  "2048"]
    vb.customize ["modifyvm", :id, "--cpus",    "2"]
  end
end
