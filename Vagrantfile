# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant plugins required:
#   vagrant plugin install vagrant-vbguest
#
Vagrant.configure("2") do |config|

    config.vm.box       = "debian/jessie64"
    config.vm.box_version = ">= 8.4.0"
    config.vm.box_check_update = false
    config.vm.host_name = "dev"
    config.vm.network "private_network", ip: "192.168.33.11"
    config.vm.network "forwarded_port", guest: 1337, host: 1337
    config.vm.network "forwarded_port", guest: 4040, host: 4040

    config.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 1
    end

    if OS.windows?
        # fix bug: http://stackoverflow.com/questions/34176041/vagrant-with-virtualbox-on-windows10-rsync-could-not-be-found-on-your-path
        config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
    else
        config.vm.synced_folder ".", "/vagrant", type: "nfs"
    end

    # If errors occur, try running "vagrant provision" manually
    # after "vagrant up"
    config.vm.provision :docker
    # To use docker_compose as a provisioning tool, install
    # vagrant-docker-compose plugin first. It should also solve the
    # "The '' provisioner could not be found." error:
    # $ vagrant plugin install vagrant-docker-compose
    config.vm.provision :docker_compose,
        rebuild: true,
        run: "always",
        yml: "/vagrant/docker-compose.yml",
        env: {
                APP_ID: "MyAppID",
                MASTER_KEY: "MyMasterKey",
                PARSE_DASHBOARD_ALLOW_INSECURE_HTTP: "1",
                SERVER_URL: "http://localhost:1337/parse"
            }
end


#OS http://stackoverflow.com/questions/26811089/vagrant-how-to-have-host-platform-specific-provisioning-steps
module OS
    def OS.windows?
        (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    end

    def OS.mac?
        (/darwin/ =~ RUBY_PLATFORM) != nil
    end

    def OS.unix?
        !OS.windows?
    end

    def OS.linux?
        OS.unix? and not OS.mac?
    end
end
