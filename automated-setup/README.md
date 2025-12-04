Automated Setup (Vagrant + Bash Provisioning)

This directory contains the fully automated provisioning workflow for the VProfile application using Vagrant, VirtualBox, and Bash scripts.
It recreates a complete multi-tier environment on a local machine, matching the architecture used in the manual installation process.

The automated setup provisions all required VMs, installs dependencies, configures services, and deploys the VProfile application without manual intervention.

#1. Overview

The automation includes:

🔹 A multi-VM Vagrantfile that defines the infrastructure layout

🔹 Individual provisioning scripts for each service

🔹 Automated installation and configuration of:

      > MySQL/MariaDB

      > Memcached

      > RabbitMQ

      > Tomcat (Application server)

      > Nginx (Web tier)

      > Backend build and deployment components (optional depending on usage)

This automated setup reflects the same provisioning order as the manual process.

#2. Repository Structure
```
automated-setup/
├── provisioning/
│   ├── backend.sh
│   ├── memcache.sh
│   ├── mysql.sh
│   ├── nginx.sh
│   ├── rabbitmq.sh
│   └── tomcat.sh
├── README.md
└── Vagrantfile
```
Purpose of each component
```
| File            | Description                                                                   |
| --------------- | ----------------------------------------------------------------------------- |
| Vagrantfile     | Defines VM topology, hostnames, network configuration, and provisioning order |
| mysql.sh        | Installs and configures MySQL/MariaDB                                         |
| memcache.sh     | Installs and configures Memcached                                             |
| rabbitmq.sh     | Installs RabbitMQ, creates users, enables plugins                             |
| tomcat.sh       | Installs Java, Tomcat, and prepares the application server                    |
| nginx.sh        | Installs and configures Nginx as reverse proxy                                |
| backend.sh      | Handles backend build tasks (Maven/Gradle) and deployment to Tomcat           |
```

#3. Prerequisites

Before running the automated setup, ensure the following software is installed on your host machine:
```
  > Vagrant (latest stable version recommended)

  > VirtualBox

  > Hardware virtualization (VT-x/AMD-V) enabled

  > Optional: vagrant-hostmanager plugin

    vagrant plugin install vagrant-hostmanager

```
#4. How to Run the Automation

From the automated-setup/ directory:

Step 1: Start the environment
```
vagrant up
```

This command:

🔹 Creates all virtual machines

🔹 Runs provisioning scripts in the correct dependency order

🔹 Configures services and networking automatically

🔹 Deploys the application (if backend script is included)

Provisioning may take several minutes depending on Internet speed and system resources.

Step 2: Verify Services (optional)

After provisioning completes:

✅ MySQL: vagrant ssh db01 → sudo systemctl status mariadb

✅ Memcached: vagrant ssh mc01 → systemctl status memcached

✅ RabbitMQ: Visit the management UI if exposed

✅ Tomcat: Access via port 8080

✅ Nginx: Access application via port 80

Step 3: Re-run provisioning (if needed)

If provisioning stops or fails mid-way:
```
vagrant provision <vm-name>
```

Or simply:
```
vagrant up
```
Vagrant is idempotent—running the same command again will continue or re-apply provisioning.

#5. Bringing Down the Environment

To stop all VMs without deleting them:
```
vagrant halt
```

To destroy all VMs completely:
```
vagrant destroy -f
```
#6. Notes

* The provisioning scripts are written to be modular and isolated per service.

* This setup is intended for local development and learning, not production.

* For production-level automation, the environment can later be migrated to:
```
    > Ansible

    > Terraform + AWS

    > Docker/Kubernetes
```
This automated setup serves as the foundation for advanced DevOps workflows in your portfolio.
