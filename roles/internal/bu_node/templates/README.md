## playbook-folder/roles/internal/blank/templates

You can place all files that use variables to substitute information during creation in this directory

This is the place where templates (i.e. files with interpolated variables goes). While this is not necessary, I often reference them using a relative path like so:

    - name: Template foo
      template:
        src: "../templates/foo.conf.j2"
        dest: /some/place/in/the/node/filesystem/foo.conf

The goal of using relative path is to be able to hit `gf` in `vim` and open the file directly. You can get rid of that and just use `src: foo.conf.j2`. It is just a readability/convenience tradeoff.

The file name I use is the intended filename at the destination, appended with `.j2` so it is clear that it is a Jinja2 template, and easier to search (find or locate).

Some folks like to replicate the destination hierarchy (e.g. `src: etc/sysconfig /network-scripts/ifcfg-ethx.cfg.j2`). This is a matter of taste, but personally I don’t see the point of having those deep hierarchies in the role if the naming is correct.
