# ansible-realms

With this Ansible template you can manage different environments within one repository. I mainly use this scheme to separate the configuration of my home network and my cloud environment. However, other uses could be to separate individual applications themselves and treat them as separate realms. This template makes that possible.

## Features
---

- Separation of different physical or logical environments
- Subdivided inventories, each with its own inventory groups and group variables
- Generic roles that can be accessed from all realms
- Roles from the ansible-galaxy can be used
- Easy use of ansible-vault to encrypt passwords and keys

## The structure
---

In the `general-roles` folder, roles can be created that are usable and reusable for the entire project. These are embeddable in every realm.

In the folder `realms` the different areas can be managed. In the example, the areas `example_1` and `example_2` exist as two different environments. Each realm has a `files` folder in which local files for the respective realm can be stored. The `templates` folder acts in the same way as the files folder. The `roles` folder contains the realm-specific roles that can be used in playbooks. A sample role `example_role` has been created. Also relevant is the file `ansible.cfg`, in which Ansible execution parameters can be configured for each realm (more on this later). An example playbook `playbook.yml` was also created. In the `requirements.yml` file (see examples), external roles and collection of the [ansible-galaxy](https://galaxy.ansible.com/) can be defined.

The `inventories` folder is used to manage the various server inventories. Two inventories `example_1` and `example_2` have been defined here as well. In the `hosts.yml` the different hosts can be created (see examples). Based on the assigned names, variables can be stored in the `host_vars` folder that only apply to a specific host.
For each inventory, group variables can be stored in the `group_vars/all` folder in the `vars.yml` file, which apply to all hosts in this inventory.

```
|-- general-roles
|   `-- example_role
|       |-- defaults
|       |-- files
|       |-- handlers
|       |-- tasks
|       `-- templates
|-- inventories
|   |-- example_1
|   |   |-- group_vars
|   |   |   `-- all
|   |   |       `-- vars.yml
|   |   |-- host_vars
|   |   `-- hosts.yml
|   `-- example_2
|       |-- group_vars
|       |   `-- all
|       |       `-- vars.yml
|       |-- host_vars
|       `-- hosts.yml
|-- realms
|   |-- example_1
|   |   |-- files
|   |   |-- roles
|   |   |   `-- example_role
|   |   |       |-- defaults
|   |   |       |-- files
|   |   |       |-- handlers
|   |   |       |-- tasks
|   |   |       `-- templates
|   |   |-- templates
|   |   |-- ansible.cfg
|   |   |-- playbook.yml
|   |   `-- requirements.yml
|   `-- example_2
|       |-- files
|       |-- roles
|       |   `-- example_role
|       |       |-- defaults
|       |       |-- files
|       |       |-- handlers
|       |       |-- tasks
|       |       `-- templates
|       |-- templates
|       |-- ansible.cfg
|       |-- playbook.yml
|       `-- requirements.yml
|-- pass
```

## Install roles from ansible-galaxy
---

In order to use roles from ansible-galaxy, it must first be added to the `requirements.yml` file (see examples). In the example, we want to install the collection [community.docker](https://docs.ansible.com/ansible/latest/collections/community/docker/index.html) and the role [geerlingguy.docker](https://galaxy.ansible.com/geerlingguy/docker) as dependencies.
First you have to change to the directory of the respective realm where the `requirements.yml` file is located. The installation is done with the following command:

`ansible-galaxy install -r requirements.yml`

The selected roles are now stored in the `general-roles` directory

##