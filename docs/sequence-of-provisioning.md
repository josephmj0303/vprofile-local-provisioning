# Sequence of Provisioning

This document explains the exact order in which the infrastructure components are provisioned, both in manual setup and automated Vagrant-based setup.

The sequence is based on service dependencies defined in the VProfile architecture.

---

## 1. Why Order Matters

Tomcat (the application tier) depends on:

- MySQL (database)
- Memcached (cache)
- RabbitMQ (message broker)

Nginx (web tier) depends on Tomcat to be available.

For this reason, provisioning must follow a strict dependency chain.

---

## 2. Provisioning Order

### **1. Database: MySQL / MariaDB**
- First service to be brought up
- Required by Tomcat for application initialization
- Runs on db01

### **2. Cache Server: Memcached**
- Must start before the application to prevent connection failures
- Runs on mc01

### **3. Message Broker: RabbitMQ**
- Needed for asynchronous operations in the app
- Runs on rmq01

### **4. Application Server: Tomcat**
- Deploys the VProfile web application
- Connects to DB, Cache, and MQ
- Runs on app01

### **5. Web Server: Nginx**
- Final component to be provisioned
- Reverse proxies requests to Tomcat
- Runs on web01

---
