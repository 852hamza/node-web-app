# Ansible Playbook for Docker Setup on Ubuntu

## Overview
This Ansible playbook automates the setup of Docker on an Ubuntu server. It installs necessary dependencies, adds the Docker repository and GPG key, installs Docker CE, and starts the Docker service. Additionally, it adds the specified user to the Docker group, allowing them to run Docker commands without `sudo`.

## Prerequisites
- Ansible installed on the control machine (your local machine).
- SSH access to the target server with the provided private key.
- Ensure that the `ansible_user` specified in the inventory has `sudo` privileges on the target server.

## Inventory File
The inventory file (`inventory`) specifies the target server's IP address and SSH configuration:

```ini
[app_server]
server-ip ansible_user=ubuntu ansible_ssh_private_key_file=Path/to/private/key/file
```

## Playbook Tasks
The playbook (`playbook.yml`) performs the following tasks:

1. **Update the apt package index**  
   Updates the list of available packages and their versions.

2. **Install prerequisites**  
   Installs essential packages required to set up Docker, including:
   - `apt-transport-https`: Allows APT to use repositories accessed via the HTTPS protocol.
   - `ca-certificates`: Provides SSL certificates.
   - `curl`: Used for data transfer.
   - `software-properties-common`: Adds repository management capabilities.

3. **Add Docker GPG key**  
   Adds Docker’s official GPG key, ensuring that the downloaded Docker packages are authentic.

4. **Add Docker repository**  
   Adds Docker’s official repository to the list of sources, allowing us to install the latest version of Docker.

5. **Update apt package index after adding Docker repo**  
   Updates the list of packages again to include the Docker repository.

6. **Install Docker**  
   Installs Docker CE (Community Edition) on the server.

7. **Start Docker service**  
   Starts the Docker service and enables it to start on boot.

8. **Add user to Docker group (optional)**  
   Adds the `ansible_user` (in this case, `ubuntu`) to the Docker group so that they can run Docker commands without using `sudo`.

## Running the Playbook

To run this playbook on the target server:

1. Ensure you have set up the inventory file (`inventory`) with the correct IP address and SSH key path.

2. Run the playbook using the following command:
   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

## Notes
- This playbook is designed for **Ubuntu**. It may not work as intended on other Linux distributions without modifications.
- The playbook installs Docker CE, which is suitable for general purposes but not for production environments where Docker EE may be required.
- The playbook assumes the `ansible_user` has `sudo` privileges.

## Troubleshooting
If you encounter permission errors, make sure:
- The `ansible_user` specified in the inventory has `sudo` privileges.
- The SSH private key path in the inventory file is correct and accessible.

## License
This playbook is provided "as is" without warranty of any kind.
