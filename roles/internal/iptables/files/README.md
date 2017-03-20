#- name: port 80/443 open for http/s
#  iptables:
#    chain: INPUT
#    protocol: tcp
#    destination_port:
#    ctstate: NEW,ESTABLISHED
#    jump: ACCEPT
#    state: present
#  with_items:
#    - "22"
#    - "80"
#    - "443"
#  notify:
#    - Save iptables
#    - Restart iptables
# playbook-folder/roles/internal/blank/files/

This directory contains regular files that need to be transferred to the hosts you are configuring for this role. This may also include script files to run.

These files do not require jinja interpolation, and can be copied as-in onto the remote nodes.

# copy

A bit of info on copy and directories. Look a the trailing slash just like rsync. Therefore to transfer a folder named `Blog`:

    - name: Copy a folder named Blog/ to /var/www/
      src: "../files/Blog"
      dest: /var/www/

But to transfer the _contents_ of the folder named `Blog`:

    - name: Copy the contents of a folder named Blog to /var/www/
      src: "../files/Blog/"
      dest: /var/www/

One little slash is all it takes. I wouldn't be surprised if this was the same for `fetch` too.
