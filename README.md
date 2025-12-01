VProfile Local Provisioning (Vagrant + Bash + VirtualBox)

A fully reproducible local multi-VM DevOps environment for the VProfile application, provisioned manually and automatically using Vagrant, VirtualBox, and Bash scripts.

This project recreates a complete multi-tier architecture entirely on your local machine—no cloud, no external services.

📌 Project Overview

The VProfile application is a multi-tier Java web application using:

Tier	Technology
Web Tier	Nginx (reverse proxy)
App Tier	Tomcat 9
Cache Tier	Memcached
Message Broker	RabbitMQ
Database	MySQL / MariaDB

This repository includes:

Architecture diagrams

Manual setup procedures for each VM

Automated provisioning using Vagrant + shell scripts

Service configuration examples

Sequence of provisioning (DB → Cache → MQ → App → Web)

🏗️ Architecture

The architecture (from the project diagrams and the PDF setup guide ) follows this structure:

            Users
              |
     +----------------+
     |   Load Balancer|
     +----------------+
              |
           Nginx
              |
        +-----------+
        |  Tomcat   |
        +-----------+
             |  \
             |   \
       Memcached  RabbitMQ
             \      /
              \    /
              MySQL


The complete diagrams are available in:

architecture/diagrams/

📁 Repository Structure
vprofile-local-provisioning/
│
├── architecture/
│   ├── diagrams/
│   └── infra-overview.md
│
├── manual-setup/
│   ├── 01-mysql-setup.md
│   ├── 02-memcache-setup.md
│   ├── 03-rabbitmq-setup.md
│   ├── 04-tomcat-setup.md
│   ├── 05-nginx-setup.md
│   ├── code-build-deploy.md
│   └── troubleshooting.md
│
├── automated-setup/
│   ├── Vagrantfile
│   ├── provisioning/
│   │   ├── mysql.sh
│   │   ├── memcache.sh
│   │   ├── rabbitmq.sh
│   │   ├── tomcat.sh
│   │   ├── nginx.sh
│   │   └── common.sh
│   └── README.md
│
├── docs/
│   ├── port-mapping.md
│   ├── vm-details.md
│   ├── service-overview.md
│   ├── sequence-of-provisioning.md
│   └── screenshots/
│
└── README.md

🔧 Technologies Used

Vagrant – VM orchestration

VirtualBox – Virtualization

Linux (CentOS / Ubuntu) – Base OS

MariaDB (MySQL) – Database

Memcached – Caching service

RabbitMQ – Message broker

Tomcat 9 – Application server

Nginx – Reverse proxy

Bash – Provisioning scripts

Git – Version control

🚀 Manual Setup (High-Level)

From the PDF instructions (pages 3–12) , the environment is manually provisioned in this order:

MySQL – Database initialization

Memcached – Caching service

RabbitMQ – Queue/broker

Tomcat – Application deployment

Nginx – Reverse proxy configuration

Each step is fully documented in:

manual-setup/

🤖 Automated Setup (Vagrant + Bash)

The automated provisioning replicates all manual steps using:

A multi-VM Vagrantfile

Individual provisioning scripts for each service

Automatic hosts file management

Automatic network assignments

Run the full environment:

vagrant up


If provisioning stops mid-way (as mentioned in the PDF), simply run:

vagrant up


Again to resume.

📝 Screenshots

You can store visual output from:

Nginx homepage

Tomcat access

MySQL login

Memcached stats

RabbitMQ management console

App homepage

Place them in:

docs/screenshots/

📚 Learning Outcomes

By completing this project, you gain practical experience in:

Multi-VM DevOps environments

Infrastructure provisioning

Bash scripting

Service configuration

Reverse proxying

Application deployment

Debugging multi-tier apps

Automating local infrastructure

📦 Future Enhancements

This project is the foundation for:

Containerization

CI/CD (Jenkins, GitHub Actions)

Ansible provisioning

Terraform + AWS (IaC)

Monitoring and logging with ELK / Prometheus

Kubernetes deployment

🙌 Contributions

This is part of a personal DevOps learning portfolio.
Feel free to fork and improve.