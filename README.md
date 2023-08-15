# AWX in Minikube

This playbook installs AWX on a Minikube instance running on an Ubuntu/Fedora host.

## Requirements

A managed node with sufficient CPU and memory resources is necessary, at least:

* 4 Cores
* 6-8 GB RAM

Run the `check-requirements.yml` playbook to ensure that your Ansible Controller has all required Collections and Python packages installed.  

Install necessary Galaxy requirements:

```bash
ansible-galaxy collection install -r requirements.yml
```

Install necessary Python requirements:

```bash
pip3 install -r requirements.yml
```

Adjust the inventory.  
The playbook needs *sudo* permissions, either set an appropriate user or use the `bootstrap.yml` playbook which prepares the managed node.  

Run the playbook and provide a user which can access the managed node and has root permissions, a dedicated *ansible* user with sudo permissions is created and all packages are updated to latest state.  
If you want to deploy Minikube and AWX on your **local** machine, ensure that the user running the `main.yml` playbook is able to run sudo!

For example, bootstrapping a remote host:

```console
ansible-playbook bootstrap.yml -e ansible_user=<default-user> --ask-pass --ask-become-pass
```

For bootstrapping the local host, either set `--connection local` as a parameter or set `ansible_connection=local` in the `inventory.ini`

```console
ansible-playbook bootstrap.yml --connection local --ask-become-pass'
```

## Usage

After installing all requirements, adjust the necessary variables in `main.yml`:

| Variable          | Default Value | Description                                                  |
| ----------------- | ------------- | ------------------------------------------------------------ |
| exposed_node_port | 30080         | The Port for the AWX UI.                                     |
| container_runtime | podman        | The container runtime to be installed and used for minikube. |

Afterwards, run the `main.yml` playbook.

```bash
ansible-playbook main.yml
```

The playbook will output further information.
