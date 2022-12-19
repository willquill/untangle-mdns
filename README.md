# untangle-mdns

Ansible playbook for adding mDNS to your Untangle box

## Overview

Sourced from the manual instructions in [this post](https://forums.edge.arista.com/forum/community-contribution/hacks/36649-mdns-external?p=365613#post365613) from [this thread](https://forums.edge.arista.com/forum/community-contribution/hacks/36649-mdns-external).

There is some discussion of my repository [here](https://forums.edge.arista.com/forum/community-contribution/hacks/37868-i-wrote-an-ansible-playbook-to-get-mdns-working-with-untangle).

What this playbook does:

- Installs avahi-daemon with dependencies
- Configures /etc/avahi/avahi-daemon.conf
- Installs libnss-mdns for hostname resolution
- Installs [avahi-utils](https://command-not-found.com/avahi-browse) to let you use `avahi-browse` for troubleshooting
- Variables in `untangle-mdns.yml` allow you to specify which interfaces can broadcast Bonjour as Ansible variables

Outcome of playbook:

- So far, I have tested this by having my iPhone and my AppleTV on different vlans/subnets, routed through the Untangle box. I can use AirPlay to send the signal from my iPhone to the AppleTV.

**Important: You must also allow port 5353 on protocol UDP between the subnets used by the interfaces you specified in the playbook. This information is located on [this page](https://wiki.debian.org/Avahi).**

### Prepare SSH Keys and Config

Enable SSH in Untangle by following instructions [here](https://wiki.untangle.com/index.php/Enable_SSH).

First, you'll want to prepare your SSH keys.

Create a key if you don't have one already.

`ssh-keygen -t rsa -b 4096 -N '' -f ~/.ssh/id_rsa`

Add your Untangle hostname to your SSH config.

```sh
cat >> ~/.ssh/config<< EOF
Host untangle
    UserKnownHostsFile /dev/null
    StrictHostKeyChecking no
    IdentitiesOnly yes
    ConnectTimeout 0
    ServerAliveInterval 30
    IdentityFile ~/.ssh/id_rsa
EOF
```

Set up the destination for remote authentication.

`ssh-copy-id -i ~/.ssh/id_rsa.pub root@untangle`

### Install Ansible

#### macOS

Install homebrew and then run `brew install ansible`

#### openSUSE

`sudo zypper in python39`

`sudo pip3 install setuptools-rust wheel`

`sudo pip3 install --upgrade pip`

`sudo python3.9 -m pip install ansible`

`ansible --version`

#### Rocky Linux 8

`sudo dnf -y install python39`

`sudo pip3 install setuptools-rust wheel`

`sudo pip3 install --upgrade pip`

`sudo python3.9 -m pip install ansible`

`ansible --version`

### Prepare Variables

Update `untangle-mdns.yml` with your own variable values.

### Run the Playbook

`ansible-playbook untangle-mdns.yml -i hosts`

### Troubleshooting

Get Ansible facts for hosts: `ansible all -i hosts -m setup > facts.json`

Get a list of services and hosts on the network: `avahi-browse -art | less`

Get a list of all service names known to avahi-daemon: `avahi-browse --dump-db`

Display service types of services discovered on the network: `avahi-browse -kat`

## License

Distributed under the MIT License. See LICENSE for more information.
