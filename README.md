Development Starter Kit
=======================

This is a set of resources you can use to kick-start development, as well as keeping a healthy separation between development resources and production deliverables. You are welcome to clone this and run with it, however I'm mainly sharing this to provide an example of what has worked well for me when trying to keep a tidy and sane development environment.

Galaxy
------

Nothing in this starter kit is new - it's just bringing together already excellent projects. As such, we'll use ansible-galaxy to pull down the required Ansible roles - use `ansible-galaxy install -r requirements.yml -p roles/ --force` (or modify this to your requirements) and you'll be ready to run the playbook below. Remember, you can add that line to your Jenkins job when you have it up and running, to make sure third-party changes are automatically applied to your environments (you might, of course, not want to do this after all).

Playbook
--------

The development.yml in this repository runs all development environment-specific configuration. Currently it's configured to create users and add SSH keys, as well as configuring jenkins and gitlab. The beauty of this setup is that with the inventory hierachy (explained below) you only have to run the development playbook once to configure your whole pipeline, while environment-specific playbooks and group_vars still exist on a per-environment basis. Run this playbook referring to the whole inventory directory (`-i inventory`) to configure the whole pipeline at once.

Inventory
---------

Within the inventory directory exists a subdirectory, per environment, that houses a list of servers (in the `inventory` file) and a list of environment-specific variables (under the `group_vars` directory). This gives you a clean separation of environments, as well as a single root you can run pipeline-wide changes to (adding users, changing repositories, etc - things that wouldn't be done in production). The inventory should be referred to, when running environment-specific plays, at the directory level (eg `-i inventory/sandbox`) to capture that environment's group_vars in the play.

Vagrant
-------

This is an example Vagrantfile, using the yaml plugin and `vagrant/config.yaml` to stand up some example servers and run Ansible against them. Groups and group_vars are configured in the Vagrantfile.

myrepos
-------

[MyRepos](https://myrepos.branchable.com/) is an excellent tool for developers, since it easily makes sure they're looking at the latest changes (and the correct branch) for all the repositories on a project. You can configure the repositories to pull in the .mrconfig file - some examples are there for you to modify.

Upon installing the myrepos tool (`brew install mr` on a mac), running `mr co` while in the root of this repository will clone all development repos to the *repos* directory. The tool also allows you to update all repos on the fly as well as many other useful features - `man mr` for more details.
