Infrastructure Overview

This document provides a high-level overview of the local multi-VM infrastructure used to provision and run the VProfile application. The goal of this environment is to replicate a production-grade multi-tier architecture entirely on a local workstation using Vagrant, VirtualBox, and automated Bash provisioning.

The infrastructure consists of multiple VMs, each dedicated to a specific tier of the application stack, allowing clean separation of responsibilities and predictable service behavior.

🏗️ Architecture Summary

The VProfile application follows a classic multi-tier design:
```
✔ Web Tier: Nginx reverse proxy

✔ Application Tier: Tomcat 9 hosting the Java web application

✔ Cache Tier: Memcached for session and object caching

✔ Message Broker: RabbitMQ for asynchronous message handling

✔ Database Tier: MySQL/MariaDB as the persistent data store
```
This architecture mirrors the real-world deployment of a scalable Java-based application.

Visual diagrams are available under:
```
architecture/diagrams/
```

📌 Component Responsibilities

Each VM in this environment hosts a single major component to ensure isolation, easy debugging, and clean provisioning.
```
| VM Name | Service            | Technology      | Purpose                                         |
| --------| ------------------ | --------------- | ----------------------------------------------- |
| db01    | Database           | MySQL / MariaDB | Stores persistent application data              |
| mc01    | Cache              | Memcached       | Provides caching layer for improved performance |
| rmq01   | Message Broker     | RabbitMQ        | Handles background asynchronous tasks           |
| app01   | Application Server | Tomcat 9        | Runs the VProfile WAR application               |
| web01   | Web Tier           | Nginx           | Acts as reverse proxy and entry point for users |
```
All services communicate via a host-only private network defined in the Vagrantfile.

🚀 Network Layout

Each VM is provisioned with a static private IP, enabling predictable service communication.

Typical network flow:

1. Users → web01 (Nginx)
   Nginx receives all traffic and proxies requests to the application server.

2. web01 → app01 (Tomcat)
   Nginx forwards traffic to Tomcat via internal private IP on port 8080.

3. app01 → db01 / mc01 / rmq01
   Tomcat connects to:

        > MySQL (db01) for persistent storage

        > Memcached (mc01) for caching

        > RabbitMQ (rmq01) for internal message queueing

4. Internal Services
These services communicate only over the private host-only network, ensuring isolation from the external host.

A full VM network diagram is included in:
```
architecture/diagrams/vprofile-network-layout.png
```

🎯 Provisioning Workflow

Provisioning follows a strict, dependency-driven sequence to ensure all upstream components are ready before the application starts.
```
1. Database (MySQL / MariaDB)

2. Cache Server (Memcached)

3. Message Queue (RabbitMQ)

4. Application Server (Tomcat + Application Deployment)

5. Reverse Proxy (Nginx)
```
This sequence ensures:

* Tomcat does not start before MySQL, Memcached, and RabbitMQ are available

* Nginx routes traffic only after the application server is ready

The diagram representing provisioning order is located in:
```
architecture/diagrams/vprofile_provisioning-order.png
```

🤖 Automated Provisioning Overview 

Automation for this environment is implemented using:

* Vagrant for VM orchestration

* VirtualBox as the virtualization provider

* Bash scripts for provisioning each VM

The automation replicates all manual setup steps, ensuring consistency across environment rebuilds.

Structure:
```
automated-setup/
├── Vagrantfile
└── provisioning/
    ├── mysql.sh
    ├── memcache.sh
    ├── rabbitmq.sh
    ├── tomcat.sh
    ├── nginx.sh
    └── common.sh
```
The Vagrantfile defines:

* VM names

* Hostnames

* IP addresses

* Memory/CPU

* Mapped provisioning scripts

To bring up the entire environment:
```
vagrant up
```

✅ Key Ports and Endpoints  

The following ports are used by services inside the environment:
```
| Service               | Port  | Description                       |
| --------------------- | ----- | --------------------------------- |
| Nginx                 | 80    | Entry point for all user requests |
| Tomcat                | 8080  | Application server port           |
| MySQL/MariaDB         | 3306  | Database communication            |
| Memcached             | 11211 | Cache communication               |
| RabbitMQ AMQP         | 5672  | Message queue protocol            |
| RabbitMQ Admin UI     | 15672 | Web-based management console      |
```
Port mapping details are documented separately in:
```
docs/port-mapping.md
```

📁 Architecture Diagrams 

The following diagrams provide visual representation of the environment:
```
✔ vprofile-architecture.png

✔ vprofile_network-layout.png

✔ vprofile_provisioning-order.png
```
These files are located at:
```
architecture/diagrams/
```

📚 Purpose of This Infrastructure 

This Vagrant-based infrastructure is used for:
```
✔ Learning DevOps fundamentals

✔ Practicing multi-tier provisioning

✔ Reproducing production-like environments locally

✔ Understanding service dependencies

✔ Experimenting with automation techniques
```
It serves as the foundation for future projects involving:

* Ansible provisioning

* CI/CD automation

* Dockerization

* Kubernetes deployments

* Cloud infrastructure (Terraform + AWS)


🙌 Summary

This infrastructure provides a cleanly separated, fully automated multi-VM environment suitable for both learning and demonstration purposes. By replicating a real-world application architecture locally, it enables hands-on experience with provisioning, service connectivity, troubleshooting, and automation workflows.

