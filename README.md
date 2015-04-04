# Vagrant Ansible Drupal

Virtual Machine for Drupal development, built with re-usable Ansible roles.

## Requirements
* [VirtualBox](https://www.virtualbox.org)
* [Vagrant](https://www.vagrantup.com/)
* [vagrant-triggers](https://github.com/emyl/vagrant-triggers)
* [vagrant-hostmanager](https://github.com/smdahlen/vagrant-hostmanager) (optional)
* [Ansible](http://www.ansible.com) 1.8+

## Instructions

1. Clone this repository
   ```
   git clone https://github.com/pbuyle/vagrant-ansible-drupal.git
   cd vagrant-ansible-drupal
   ```
2. Copy `parameters.yml.dist` to `parameters.yml` (optional)
3. Create a `www` folder with your Drupal docroot (usage of a symlink is recommanded)
4. Start the Vagrant VM
   ```vagrant up```
5. If not using vagrant-hostmanager, edit you [hosts file](https://en.wikipedia.org/wiki/Hosts_%28file%29) to add 
   ```
   192.168.9.10	drupal.dev
   ```
6. SSH into the Vagrant VM
  ```
  vagrant ssh
  ```
7. Install your Drupal site
   ```
   drush site-install --db-url=mysqli://drupal:drupal@localhost/drupal
   ```
7. Open http://drupal.dev
8. ?
9. Profit

## TODO (Pull requests welcome)
* MariaDB and PostgreSQL support
* Memcached support 
* Redis support 
* NodeJS runtime
    * Bower
    * Grunt/Gulp
* Ruby (rbenv) support

## License

Apache v2

## Author Information

Pierre Buyle <pierre@buyle.org>