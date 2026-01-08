# Orchestra

This repository contains the necessary files and documentation to configure server with Ansible.

## Prerequisites

Before getting started, make sure you have the following:

- Ansible installed on your local

## Getting Started

To create the server and configure it with Ansible, follow these steps:

```bash
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory/hosts web.yml
```

#### Tips for Local Execution
*   **Host Key Checking**: When running playbooks against `localhost`, you can prefix the command with `ANSIBLE_HOST_KEY_CHECKING=False` to bypass SSH host key checking, which is not relevant for local connections.
*   **Become Pass (`sudo` password)**: If your user requires a password to use `sudo` for privilege escalation, you can add the `-K` (or `--ask-become-pass`) flag to your `ansible-playbook` command. This will prompt you to enter your `sudo` password.

## PostgreSQL Setup

This project uses the `geerlingguy.postgresql` role to install and manage PostgreSQL.

### 1. Install Required Collections and Roles
Before running the PostgreSQL playbook, you need to install the role and its dependent collections from Ansible Galaxy:

```bash
ansible-galaxy install geerlingguy.postgresql
ansible-galaxy collection install community.postgresql
ansible-galaxy collection install community.general
```

### 2. Configure Variables
Edit your `vars.yml` file to set the required PostgreSQL variables. At a minimum, you need `db_name`, `db_user`, and `db_password`. You can also set optional variables like `db_port`.

### 3. Run the Playbook
Execute the `postgres.yml` playbook to install and configure PostgreSQL. If your `vars.yml` is encrypted, use `--ask-vault-pass`.

```bash
ansible-playbook -i inventory/hosts postgres.yml --ask-vault-pass
```

## Adding Inventory/Hosts for Ansible

To add inventory/hosts for Ansible to work, follow these steps:

1. Open the `inventory/hosts` file in a text editor.
2. Add the IP address or hostname of the server under the appropriate group.
3. Save the file.

Now you can use Ansible to manage and configure the server using the added inventory/hosts.

## Managing Secrets with Ansible Vault

To securely manage sensitive variables like passwords and API keys, this project uses `ansible-vault`. The required variables are stored in `vars.yml`.

### 1. Create the Variables File
First, create your own `vars.yml` by copying the example file.

```bash
cp vars.yml.example vars.yml
```

### 2. Encrypt the Variables File
Use `ansible-vault encrypt` to secure your `vars.yml` file. You will be prompted to create a vault password. **Remember this password**, as you will need it to run the playbook and edit the file.

```bash
ansible-vault encrypt vars.yml
```

### 3. Edit the Encrypted File
To edit the file and add your secret values, use `ansible-vault edit`. This will temporarily decrypt the file for editing and automatically re-encrypt it when you save and close the editor.

```bash
ansible-vault edit vars.yml
```

### 4. Running the Playbook with Vault
When you run your playbook, you need to provide the vault password. Add the `--ask-vault-pass` flag to your command:

```bash
ansible-playbook -i inventory/hosts web.yml --ask-vault-pass
```

## Exposed Ports

The following table lists the default ports exposed by the services:

| Service         | Port(s)                       | Description                                                     |
| :-------------- | :---------------------------- | :-------------------------------------------------------------- |
| Jenkins         | 8080                          | Web UI                                                          |
| Kafka           | 9092, 9101                    | 9092 (Broker), 9101 (JMX for monitoring)                        |
| Kafka UI        | 8080                          | Web UI                                                          |
| Jaeger          | 16686                         | Web UI / Frontend                                               |
|                 | 14268                         | Collector (Jaeger Thrift over HTTP)                             |
|                 | 6831/udp                      | Agent (Jaeger Thrift over UDP)                                  |
|                 | 6832/udp                      | Agent (Internal)                                                |
|                 | 5778                          | Agent (Config service)                                          |
|                 | 5775/udp                      | Agent (Zipkin format)                                           |
|                 | 9411                          | Collector (Zipkin format)                                       |
|                 | 4317                          | Collector (OTLP over gRPC)                                      |
|                 | 4318                          | Collector (OTLP over HTTP)                                      |
| MinIO           | 9005, 9006                    | 9005 (API), 9006 (Console)                                      |
| Portainer       | 8000, 9000, 9443              | 8000 (Edge Agent), 9000 (HTTP), 9443 (HTTPS)                    |
| PostgreSQL      | 5432                          | Database Server                                                 |
| Redis           | 6379                          | Redis Server                                                    |
| Redis Insight   | 8001                          | Web UI                                                          |

