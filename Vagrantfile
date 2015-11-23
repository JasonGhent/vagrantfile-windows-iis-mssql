# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

if ! File.exists?('./NDP452-KB2901907-x86-x64-AllOS-ENU.exe')
  puts '.Net 4.5.2 installer could not be found!'
  puts "Please run:\n curl -O http://download.microsoft.com/download/E/2/1/E21644B5-2DF2-47C2-91BD-63C560427900/NDP452-KB2901907-x86-x64-AllOS-ENU.exe"
  exit 1
end

if ! File.exists?('./SQLEXPRWT_x64_ENU.exe')
  puts 'SQL Server installer could not be found!'
  puts "Please run:\n curl -O http://download.microsoft.com/download/0/4/B/04BE03CD-EAF3-4797-9D8D-2E08E316C998/SQLEXPRWT_x64_ENU.exe"
  exit 1
end


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ferventcoder/win2008r2-x64-nocm"
  config.vm.guest = :windows

  config.vm.provider "virtualbox" do |v|
    v.name = "Reagan (vagrant)"
    v.memory = 4096
    v.cpus = 2
  end

  config.vm.communicator = "winrm"

  config.vm.network "private_network", ip: "192.168.123.123"
  config.vm.network :forwarded_port, guest: 1025, host: 1025
  config.vm.network :forwarded_port, guest: 3389, host: 1234
  config.vm.network :forwarded_port, guest: 5985, host: 5985, id: "winrm", auto_correct: true

  # .NET 4.5
  config.vm.provision :shell, path: "vagrant-scripts/install-dot-net.ps1"
  config.vm.provision :shell, path: "vagrant-scripts/install-dot-net-45.cmd"

  # Database
  config.vm.provision :shell, path: "vagrant-scripts/install-sql-server.cmd"
  config.vm.provision :shell, path: "vagrant-scripts/configure-sql-server.ps1"

  # IIS
  config.vm.provision :shell, path: "vagrant-scripts/install-iis.cmd"
  config.vm.provision :shell, path: "vagrant-scripts/install-aspnet.cmd"

  #Create Website
  config.vm.provision :shell, path: "vagrant-scripts/copy-website.ps1"
  config.vm.provision :shell, path: "vagrant-scripts/creating-website-in-iis.cmd"
  config.vm.provision :shell, path: "vagrant-scripts/setup-permissions-for-website-folder.ps1"
end
