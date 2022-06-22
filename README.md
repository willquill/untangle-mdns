# untangle-mdns

Ansible playbook for adding mDNS to your Untangle box

## Overview

Sourced from the manual instructions [here](https://forums.untangle.com/hacks/44147-mdns-external.html#post249705).

What this playbook does:

- Installs avahi-daemon with dependencies
- Configures /etc/avahi/avahi-daemon.conf
- Allows you to specify which interfaces can broadcast Bonjour as Ansible variables

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

`ansible-playbook -i hosts -l untanglenodes untangle-mdns.yml`

## License

Distributed under the MIT License. See LICENSE for more information.
