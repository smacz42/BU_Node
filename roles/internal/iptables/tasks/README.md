## playbook-folder/roles/internal/blank/tasks

This directory contains all of the tasks that would normally be in a playbook. These can reference files and templates contained in their respective directories _without_ using a path.

## main.yml

This file is the tasks entry point. However, it should be mostly empty. Why? Because you want to use Ansible tags. Tags are a great way to limit task execution for an Ansible run, where only tagged tasks are run.

For instance, in a playbook that deploys your application, you could choose to run only tasks regarding nginx.

The problem is that tagging every task in `main.yml` would be cumbersome, error prone, and clutter the code unnecessarily.

The best way to tag all your tasks is to include your real task file from `tasks/main.yml` and tag the whole file:

    - include: foobar.yml tags=foobar

Here, I name the real task file foobar.yml with the same name as the role (quite handy with `find` or `locate`; no need to guess which `main.yml` you are looking for) and apply the tag `foobar` to all tasks in the role.

You can repeat this if you have a big list of tasks and want to split them in several files. You could, for instance, separate configuration and installation matters, and add another specific tag for each of them:

    - include: foobar-install.yml tags=foobar,foobar_install
    - include: foobar-config.yml tags=foobar,foobar_config

Here I added two tags to the installation part (`foobar` and `foobar_install`), and two for the configuration part (`foobar` and `foobar_config`).

Note that the `_` between, for instance, foobar and config has no meaning. Ansible treats tags as dumb strings. It is just a personnal convention (Redis like) for refining tags.

With this setup, you could run only the configuration part of your role by issuing:

    ansible-playbook playbook.yml -t foobar_config

The `-t` and `-l` combination is a very powerful weapon to target a specific host with a precise change (think of this as pointing to a matrix cell targetting host (i.e. row) and tag (i.e. column)).

## additional `yml` files

The main.yml files are the ones picked up automatically by Ansible, but we can include additional files easily by using the include functionality.

If we had an additional task file used to configure SSL for some of our hosts located at tasks/ssl.yml, we could call it like this:

    tasks:
      - include: roles/nginx/tasks/ssl.yml

## check_vars.yml

I use this file to ensure that required variables are defined.

    #
    # Checking that required variables are set
    #
    name: Checking that required variables are set
    fail: msg="{{ item }} is not defined"
    when: not {{ item }}
    with_items:
        - foobar_database
        - foobar_deploy_user

Then, include this file in tasks/main.yml:

    - include: check_vars.yml tags=foobar,foobar_check,check
    - include: foobar.yml tags=foobar

# Includes content

There are a couple typical task files that are included. Typically this includes a subset of:

* blank_install
* blank_config
* blank_update

This is for 1) Installing the program. 2) configuring the program, and 3) Updating the program with userdata/media. Don't mistake 'update' with 'update the program'. The install should have `state: latest` so that when the install task list is ran, it checks it against the latest version.

The tags are all up there too. There are typically three per task file. For instance:

    - include: blank_install.yml
      tags: [blank,blank_install,install]

This is the preferred syntax (using newlines of YAML instead of equal signs for assignation).
