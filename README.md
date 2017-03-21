# BU_Node Playbook

Role to set up [Bitcoin Node Stats](https://github.com/bartromgens/bitcoinnodestats) web service on top of setting up a Bitcoin Unlimited node.

With the recent [DDoS attacks](https://www.reddit.com/r/btc/comments/60cxj1/bitcoinunlimitedinfo_under_ddos_download_bu_from/) that came along with the surge in popularity, every little bit of decentralization helps.

## Invocation

```
$ git clone https://github.com/smacz42/BU_Node && cd BU_Node
$ ansible-playbook site.yml -i inventory/dev --vault-password-file=../vault_pass.txt
```

See below for details about the 'vault'.

## Operating System

This has only been tested on CentOS7 x86_64.

# Roles

## Required Roles

* `smacz42.common`
* `smacz42.iptables`
* `smacz42.bu_node`
* `smacz42.httpd`

## Included Roles

ATM this Playbook has all necessary roles included inside of `roles/internal`. (See TODO below)

# Variables

## Testnet

This Playbook allows running on both the testnet and the mainnet. This is governed by the `bu_node_testnet` variable. Change this to `False` to run on the mainnet.

## Custom Subdirectory

This Playbook allows the web service to reside on a custom subdirectory of a webpage. This is set by the `bu_node_custom_subdir` variable.

## Binary Downloads

This Playbook assumes that the latest binary release is bitcoinUnlimited-1.0.1.1, and that it is being run on a 64-bit base image. During testing, [BitcoinUnlimited.info](https://www.bitcoinunlimited.info/) came under a DDoS attack, and I was informed that the tarball of the binaries could also be found at their [github repo](https://github.com/BitcoinUnlimited/BitcoinUnlimitedWebDownloads). So in this Playbook we look to both of them in turn, only failing if both of them are unreachable.

## RPC Password

There is a (probable) limitation where the btc password cannot include `/` or `:`. For this reason, as long as it is sufficiently long (I recommend [KeepassX](https://www.keepassx.org/) for keeping track of passwords) a purely alphanumeric password should suffice. Also, I hadn't looked at the code of the RPC service, but I would assume that you'd not want to expose port 8332 and the RPC service to the open internet as the password is likely sent in-the-clear.

The password itself is stored in the vault.yml, however, the password file is not included in this Playbook (for obvious reasons). Create a file `vault_pass.txt` in the same directory that you download the Playbook to and place a password in there. That is able to handle all password requests such as:

* `ansible-vault edit inventory/dev/group_vars/all/vault.yml --vault-password-file=../vault_pass.txt`
* `ansible-playbook site.yml --vault-password-file=../vault_pass.txt`

Currently the variables required in the vault are:

* `vault_bu_node_rpc_server_password`
* `vault_bu_node_stats_secret_key`

## Splitting the Webserver and the Full Node

This playbook makes it possible to split up the webserver from the bitcoin unlimited full node. This can hypothetically scale to many bitcoin nodes, but has only been tested with one of each host type.

To achieve this, comment out two lines in the Playbook:

```
vars_files:
  - vars/single_host.yml
```

Then, go into the inventory's `group_vars` and adjust the IP address variables under `bu_node_rpc_server` accordingly. Finally set the hostnames under `bu_nodes` and `webservers` accordingly.

Right now, `inventory/prod` is configured to be a single host setup, and the `inventory/dev` is configured to be a multi-host setup.

# Network

This doesn't touch any infrastructure, so any port forwarding (port 8333) would have to be done on whatever router is being used. Don't be a leech!

# TODO

* Configure `ip6tables` instead of simply disabling it.
* Automatically handle single node vs split nodes variables configuration
* Put roles up on galaxy.ansible.com and include them in `requirements.txt` instead of `roles/internal`
* Split `bu_node_node` out to separate `bu_node` as well as `bu_node_webservers` into `bitcoinnodestats` separate web service.
