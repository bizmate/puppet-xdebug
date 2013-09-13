puppet-xdebug
=============

Puppet class for installing XDEBUG PHP Extension - FOR VAGRANT

Installs XDEBUG Support.
Used at (tested with)
 - git@github.com:openeyes/OpenEyes.git (develop branch)

Example usage in your manifest:

    import 'classes/*' # notice i m loading the classes from this folder

    class amp {
      exec { 'apt-update':
        command => '/usr/bin/apt-get update',
      }

      include apache2
      include mysql
      include php5
      include openeyes
      include xdebug #this is all you need to add to your current manifest, the other lines are for example purposes
    }

    include amp

Notes about your VAGRANT file

you need to have a fixed IP so that xdebug can talk back to your host machine/to your IDE (PhpStorm is better)
maybe you can use the NAT ip 10.0.2.2 but it didnt work for me

EXAMPLE OF VAGRANT FILE (based on Vagrant 1.3.1)

    # -*- mode: ruby -*-
    # vi: set ft=ruby :

    Vagrant.configure("2") do |config|
      config.vm.box = "precise64"
      config.vm.box_url = "http://files.vagrantup.com/precise64.box"

      config.vm.network :forwarded_port, host: 8888, guest: 80
      config.vm.network :forwarded_port, host: 3333, guest: 3306
      config.vm.network "private_network", ip: "192.168.50.4"
      config.vm.synced_folder "./", "/var/www", id: "vagrant-root", :mount_options => ["dmode=777,fmode=666"]


      config.vm.provision :puppet do |puppet|
        puppet.manifests_path = "puppet"
      end
    end



Advanced configuration:

    xdebug::config { 'default':
            remote_host => '192.168.50.1', # Vagrant users can specify their address
            remote_port => '9000', # Change default settings
            remote_autostart => '1',
            remote_enable => '1',
            profiler_enable => '1',
            profiler_output_name => 'xdebug.log',
            idekey => 'PHPSTORM',
            #remote_handler => 'dbgp',
    }

Author: Diego Gullo

GitHub: git@github.com:bizmate/puppet-xdebug.git

Contact : www.bizmate.biz

Changelog:



ToDo:
- structure it as a puppet module
