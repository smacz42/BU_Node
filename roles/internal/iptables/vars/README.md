## playbook-folder/roles/internal/blank/vars

Variables for the roles can be specified in this directory and used in your configuration files

It is sometimes difficult to grok the difference between `vars/main.yml` and `defaults/main.yml`. After all, they both contain variable assignements.

I do not always use a `vars/main.yml`, but when I do, I put “constants like” variables in it. These are variables that are not intended to be overriden.

For instance the github repository for a particular piece of code (e.g. your web application) will certainly go there. However, the version you want to deploy won’t.

All in all, it is just a mechanism to take those values out of tasks files readability and role life cycle.

While it is possible to configure default parameters for a role through a vars/main.yml file, this is usually not recommended, because it makes the details of your configuration reside within the roles hierarchy.

Usually, you want to specify your details outside of the role so that you can easily share the role structure without worrying about leaking information. Also, variables declared within a role are easily overridden by variables in other locations, so they are not very strong to begin with.

## tl;dr

This is for "IF RHEL THEN ELIF DEBIAN THEN FI" stuff.
