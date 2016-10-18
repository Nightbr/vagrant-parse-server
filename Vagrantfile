# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant plugins required:
#   vagrant plugin install vagrant-vbguest
#   vagrant plugin install vagrant-docker-compose
#   vagrant plugin install vagrant-hostmanager
Vagrant.configure("2") do |config|

    config.vm.box = "debian/jessie64"
    config.vm.box_version = ">= 8.4.0"
    config.vm.box_check_update = false
    # configure hosts
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.manage_guest = false
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true
    config.vm.define 'default' do |node|
       node.vm.hostname = 'parse-dev-hostname'
       node.vm.network :private_network, ip: '192.168.33.11'
       node.hostmanager.aliases = %w(dash.parse.dev api.parse.dev)
    end
    # Parse API port
    #config.vm.network "forwarded_port", guest: 1337, host: 1337
    # Parse Dashboard port
    #config.vm.network "forwarded_port", guest: 4040, host: 4040

    config.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 1
    end

    if OS.windows?
        # fix bug: http://stackoverflow.com/questions/34176041/vagrant-with-virtualbox-on-windows10-rsync-could-not-be-found-on-your-path
        config.vm.synced_folder "docker-parse-server/", "/vagrant", type: "virtualbox"
    else
        config.vm.synced_folder "docker-parse-server/", "/vagrant", type: "nfs"
    end

    # If errors occur, try running "vagrant provision" manually
    # after "vagrant up"
    config.vm.provision :docker
    # To use docker_compose as a provisioning tool, install
    # vagrant-docker-compose plugin first. It should also solve the
    # "The '' provisioner could not be found." error:
    # $ vagrant plugin install vagrant-docker-compose
    # HTTPS works only for production and valid domain name (see let's encrypt for more informations)
    config.vm.provision :docker_compose,
        rebuild: true,
        run: "always",
        yml: "/vagrant/docker-compose-le.yml",
        env: {
                APP_ID: "MyAppID",
                MASTER_KEY: "MyMasterKey",
                JAVASCRIPT_KEY: "JSAppKey",
                PARSE_SERVER_VIRTUAL_HOST: "api.parse.dev",
                PARSE_SERVER_LETSENCRYPT_HOST: "api.parse.dev",
                PARSE_SERVER_LETSENCRYPT_EMAIL: "parse.dev@gmail.com",
                SERVER_URL: "http://api.parse.dev/parse",
                PARSE_DASHBOARD_ALLOW_INSECURE_HTTP: "1",
                PARSE_DASHBOARD_VIRTUAL_HOST: "dash.parse.dev",
                PARSE_DASHBOARD_LETSENCRYPT_HOST: "dash.parse.dev",
                PARSE_DASHBOARD_LETSENCRYPT_EMAIL: "parse.dev@gmail.com",
                USER1: "parseadmin",
                USER1_PASSWORD: "adminpass"
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
