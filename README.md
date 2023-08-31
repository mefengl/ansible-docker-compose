# Ansible-Docker-Compose Example

This repository contains a simple example that demonstrates how to set up an Ansible control node and a managed node using Docker Compose. The Ansible control node runs a "Hello, World!" playbook on the managed node.

## Prerequisites

- Docker
- Docker Compose

## Quick Start

### Clone the Repository

Clone this repository to your local machine.

```bash
git clone https://github.com/mefengl/ansible-docker-compose.git
```

Navigate to the project directory.

```bash
cd ansible-docker-compose
```

### Build and Run Docker Containers

Run the following commands to build and start the Docker containers.

```bash
docker-compose build
docker-compose up -d
```

### Get Managed Node IP

Run this command to get the IP address of the managed node.

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -q --filter "name=ansible_managed")
```

Note down the IP address for the next step.

### Run Ansible Playbook

1. Connect to the Ansible control node:

    ```bash
    docker exec -it $(docker ps -q --filter "name=ansible_control") /bin/bash
    ```

2. Create an inventory file (`hosts.ini`) with the IP address obtained in the previous step and specify the local connection plugin:

    ```bash
    echo -e "[managed]\n<ip_address> ansible_connection=local" | tee hosts.ini
    ```

    Replace `<ip_address>` with the actual IP address.

3. Run the Ansible playbook:

    ```bash
    ansible-playbook -i hosts.ini /ansible/hello-world.yml -u root
    ```

## Expected Output

You should see the "Hello, World!" message output from the Ansible playbook, indicating it ran successfully on the managed node.
