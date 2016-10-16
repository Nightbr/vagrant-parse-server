# vagrant-parse-server

Create a vagrant environment for parse-server provisioned by docker

* [docker-parse-server](https://github.com/yongjhih/docker-parse-server)

## Requirements

* Vagrant (virtualbox) ~1.8
* Vagrant plugins
    * vagrant-vbguest
    * vagrant-docker-compose

## Quick start

    git clone https://github.com/Nightbr/vagrant-parse-server.git
    cd vagrant-parse-server/
    vagrant up

## Services

* Parse dashboard: http://192.168.33.11:4040/
    * username: `parseadmin`
    * password: `adminpass`
* PARSE API: http://192.168.33.11:1337/parse/
    * X-Parse-Application-Id: `MyAppID`
    * X-Parse-Master-Key: `MyMasterKey`

## ToDo

- [ ] more configuration, more parse services (social, analytics, ...)
- [ ] vagrant plugin vagrant-hosts (manage hostname for the env)
- [ ] SSL with letsencrypt
- [ ] more documentation (configure parse SDK, ...)
- [ ] update parse-dashboard to the latest version
