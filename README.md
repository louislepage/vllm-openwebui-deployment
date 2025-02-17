# Proof of concept for vllm-server-deployment
This repo holds the ansible playbook to install vllm and all required packages on an ubuntu server.
It will probably not run out of the box on your machine, but it should be a good starting point.

## Requirements
- Ansible
- Remote Ubuntu Server with 24.04
- SSH access to the server

## Preparation
1. Clone this repo
2. Update the `inventory/hosts.ini` file with the IP address of your server.
3. Add a huggingface API key to the `vars/vars.yml` file.

## Running the playbooks

### Setup CUDA
This adds the nvidia keyring to allow installation of cuda packages and installs cuda toolkit. 
We do not need the toolkit on the host, buts its nice to have for debugging and testing.
```bash
ansible-playbook playbooks/setup-cuda-toolkit.yml
```

### Setup Docker for GPU Use
This playbook will install docker from the docker repo and nvidia-container-toolkit on the host.
To install nvidia-container-toolkit, the previous playbook must have been run to add the nvidia keyring.
```bash
ansible-playbook playbooks/setup-docker.yml
```


### Run vLLM and OpenwebUI in docker.
This playbook uses the compose file at `docker-compose/compose.yml` to run vllm and openwebui in docker.
It copies the compose file to the host, generates a random token for the vllm API and starts the containers.
```bash
ansible-playbook playbooks/start-vllm.yml
```

## vLLM and OpenwebUI Configuration
Both vLLM and OpenwebUI are configured in the `docker-compose/compose.yml` file.
