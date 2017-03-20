# playbook-folder/roles/internal/blank/handlers

All handlers that were in your playbook previously can now be added into this directory

This is where you define handlers that get notified by tasks. Handlers are just standard tasks. You can use `include` in `main.yml` if you want to separate handlers (for different OSes versions for instances), but try to keep the file number as low as possible so you don’t end up hunting down stuff everywhere.

If your handler restarts any service, you have to make sure that the service config file is valid before attempting to restart it. Some daemons allow this (e.g. nginx, haproxy, apache). If your service does not, provide some fallback mechanism. You don’t want your playbook to screw up your running system because you typoed a configuration variable. See the validate option in the template module.

Note that handlers are just standard tasks.

## Services

Handlers are typically used to restart services after configuration has changed. That being the case, there will not be many handlers for [install] tags, as that is setup before the actual runtime, and after that, nothing is being changed - moreso confirmed by running [install].

Once things start changing - say in [config] or [update] is where you'll see more and more handlers being used.
