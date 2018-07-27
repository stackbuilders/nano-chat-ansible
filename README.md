![Stack Builders](https://github.com/stackbuilders/nano-chat/raw/master/sb.png)

# Ansible provisioning

## Set up your SSH keys

Verify that your private and public key are located in the default directory `~/.ssh`.
The public key is used by Vagrant to establish the connection between your machine and the virtual machine.

If you don't have a SSH key you can read [how to generate one](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).

You can check out the configuration on the "General Vagrant VM configuration." section of the [Vagrantfile](Vagrantfile).

## Getting started with Vagrant

Download the VM images and create two guest machines, one in 192.168.1.10 and the other in 192.168.1.20.

```bash
vagrant up
```

Check if everything is working fine in vagrant

```bash
vagrant status
```

Login to *webserver* machine

```bash
vagrant ssh webserver
```

Check if SSH works properly

```bash
ssh vagrant@192.168.1.10
```

You have your servers up and running but with no apps.

## Provisioning with Ansible

### Install requirements on target server

1. Verify connection through Ansible

    ```bash
    ansible -m ping all
    ```

    When you receive a pong request, it means that Ansible can connect with the remote servers.

1. Install galaxy requirements

1. Add your database server to the `hosts` file.

1. Add the following apps to the `default-apps` role
    - htop
    - tree  
    - net-tools
    - vim
    - ufw
    - make

1. Open the ports 80 and 443 on the `networking` role.

1. Add `default-apps` and `networking` roles to **webserver playbook**

1. Run the playbook

    ```bash
    ansible-playbook playbooks/webserver_playbook.yml
    ```

### Prepare a deployment

1. Clone the Node.js app repository on the `deploy-app` role:

    ```
    https://github.com/stackbuilders/nano-chat.git
    ```
    Make sure the destination of the application is the `node_app_location` directory.

1. Start the Node.js application on the `deploy-app` role by executing:

    ```
    make start -C {{ node_app_location }}
    ```
1. Run the playbook

    ```bash
    ansible-playbook playbooks/deploy_playbook.yml
    ```

    Your app is now provisioned. Test it on [http://192.168.1.10](http://192.168.1.10)

    ![Deployed](meme.gif)

### Using a database

1. Run the database playbook to provision the database server.

    ```bash
    ansible-playbook playbooks/database_playbook.yml
    ```

1. Install `sequelize-cli` node module on the `deploy-app` role

1. Run Sequelize commands on the `{{ node_app_location }}/backend` directory:

    ```bash
    sequelize db:migrate
    sequelize db:seed:all
    ```

1. Define a environment variable `DB_CONN_NANOCHAT` with value of `postgresql://postgres@192.168.1.20:5432/nanochat` **deploy playbook**

1. Test the application's endpoint to retrieve some data from database on [http://192.168.1.10/api/users](http://192.168.1.10/api/users)

## License

MIT, see the [LICENSE](LICENSE) file.
