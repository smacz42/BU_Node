## playbook-folder/roles/internal/blank/meta

This directory can contain files that establish role dependencies. You can list roles that must be applied before the current role can work correctly.

## meta/main.yml

This metadata has (AFAIK) only two variables:

* galaxy_info: meta information for galaxy about your role. You just don’t need this if you don't intend to push your role to Galaxy.
* dependencies: what roles this role depends on.

The latter is of utmost importance, and setting it right deserves a blog post on it’s own. Until then, the rule of thumb to remember is to only include compulsory role dependencies for the target host.

This means that adding nginx dependency in a php-fpm role sounds perfectly reasonable1. However, adding a mysql dependency to your web application role is not, because mysql can be deployed on another server.

Below is the template for this file. It is not a file in itself b/c ansible complains when it is empty and I don't feel like filling it out until it's time to publish the task or put it in a VCS anyways:

    ---
    # Metadata for role

    - galaxy_info:

    - dependencies:
