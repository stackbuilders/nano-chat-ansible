![Stack Builders](https://github.com/stackbuilders/nano-chat/raw/master/sb.png)

# Ansible provisioning

## Set up your SSH keys

Verify that your private and public key are located in the default directory `~/.ssh`.
The public key (z) is used by Vagrant to establish the connection between your machine and the virtual machine.

You can read the configuration on the "General Vagrant VM configuration." section of the [Vagrantfile](Vagrantfile).

## Getting started with Vagrant


Download the VM images and create two guest machines, one in 192.168.1.10 and the other in 192.168.1.20.

```bash
vagrant up
```

Check if everything is working fine in vagrant

```bash
vagrant status
```

Enter to one of the machines

```bash
vagrant ssh webserver
```

Check if SSH works properly (and add the guest key to the list of known hosts)

```bash
ssh vagrant@192.168.1.10
```

You have your servers up and running but with no apps.

## Provisioning with Ansible

Verify connection through Ansible

```bash
ansible -m ping all
```

When you receive a pong request, it means that Ansible can connect with the remote servers.

Next, install the galaxy requirements

```bash
ansible-galaxy install -r requirements.yml
```

Run the playbooks

```bash
ansible-playbook playbooks/webserver_playbook.yml
ansible-playbook playbooks/database_playbook.yml
ansible-playbook playbooks/deploy_playbook.yml
```

Your app is now provisioned. Test it on `http://192.168.1.10`
![Deployed](meme.gif)

## License

MIT, see the [LICENSE](LICENSE) file.
