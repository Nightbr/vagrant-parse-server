# vagrant-parse-server

Create a vagrant environment for parse-server provisioned by docker

* [docker-parse-server](https://github.com/yongjhih/docker-parse-server)

## Requirements

* Vagrant (virtualbox) ~1.8
* Vagrant plugins
    * vagrant-vbguest
    * vagrant-docker-compose
    * vagrant-hostmanager

## Quick start

    git clone https://github.com/Nightbr/vagrant-parse-server.git
    cd vagrant-parse-server/
    vagrant up

## Services

* Parse dashboard: <http://dash.parse.dev/>
    * username: `parseadmin`
    * password: `adminpass`
* PARSE API: <http://api.parse.dev/parse/>
    * X-Parse-Application-Id: `MyAppID`
    * X-Parse-Master-Key: `MyMasterKey`

## ToDo

- [X] vagrant plugin vagrant-hostmanager (manage hosts for the env)
- [X] SSL with letsencrypt [ONLY on PROD with valide domain name]
- [ ] expose parse cloud folder
- [ ] more configuration, more parse services (social, analytics, ...)
- [ ] more documentation (configure parse SDK, ...)
- [ ] update parse-dashboard to the latest version
