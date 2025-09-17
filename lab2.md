PaaS
```mermaid
graph TB
    subgraph User["End User"]
        UI[React Frontend in Browser]
    end

    Firewall[Application Firewall]
    LoadBalancer[Cloud Load Balancer]

    subgraph PaaS["PaaS Setup (Azure)"]
        AppServer[Flask Web Server]
        Database[(Managed Postgres Database)]
    end

    UI --> Firewall --> LoadBalancer --> AppServer
    AppServer --> Database
```
Iaas
```mermaid
graph TB
    subgraph User["End User"]
        UI[React Frontend in Browser]
    end

    PerimeterFW[Network Firewall]
    CloudLB[Virtual Load Balancer]

    subgraph IaaS["IaaS Setup (AWS EC2, Azure VM, GCP Compute)"]
        VM1[VM running Flask + React]
        InternalFW[Internal Firewall]
        VM2[(VM running Postgres Database)]
    end

    UI --> PerimeterFW --> CloudLB --> VM1
    VM1 --> InternalFW --> VM2
```
On-Prem
```mermaid
graph TB
    subgraph User["End_User"]
        UI[React Frontend in Browser]
    end

    OuterFW[Perimeter Firewall]
    HardwareLB[On-Prem Load Balancer]

    subgraph OnPrem["On-Premise Setup (Data_Center)"]
        Server1[Physical Server: Flask + React]
        InnerFW[Internal Firewalls]
        Server2[(Physical Server: Postgres Database)]
    end

    UI --> OuterFW --> HardwareLB --> Server1
    Server1 --> InnerFW --> Server2
```
# Web Application Deployment Guide

The document describes how a web app consisting of a **React frontend**, **Flask backend**, and **postgres database** can be deployed using different infrastructure models: **IaaS**, **PaaS**, and **On-Premises**.

---

## 1. Deployment Using IaaS (Infrastructure as a Service)

In an IaaS deployment, virtual machines (VMs) are provisioned in a cloud environment such as AWS EC2, Azure Virtual Machines, or Google Cloud Compute Engine.

- The **React frontend** runs in the user's browser, while the **flask backend** is hosted on a Virtual Machine  
- The **Postgres database** is hosted on a separate VM to isolate data and improve performance.  
- Security is implemented using **firewalls** at both the network perimeter and internally between the web server and database.  
- A **load balancer** distributes user traffic across one or more VMs running the web server, providing better availability and scalability.

**Key Points:**  
- The user manages the operating system, security patches, and software stack.  
- Offers flexibility to configure servers, network rules, and scaling policies manually.  
- Ideal for teams that need full control over infrastructure while leveraging cloud resources.

---

## 2. Deployment Using PaaS (Platform as a Service)

In a PaaS deployment, the cloud provider manages most of the infrastructure, allowing developers to focus on the application and data. Providers include Heroku, Render, and Azure App Service.

- The **React frontend** runs in the user's browser.  
- The **Flask backend** is deployed as a managed application service.  
- **Postgres** is provided as a managed database service by the cloud provider.  
- **Application-level firewalls** protect the services, while a **cloud load balancer** automatically distributes incoming traffic.

**Key Points:**  
- The cloud provider handles server management, operating system updates, scaling, and high availability.  
- Users focus on maintaining the application code and database configuration.  
- Ideal for rapid deployment, simplified scaling, and reduced operational overhead.

---

## 3. Deployment Using On-Premises Infrastructure

In an On-Premises setup, the application is hosted on physical servers located in the organization’s own data center.

- The **React frontend** runs in the user's browser.  
- The **Flask backend** and React components run on a dedicated physical server.  
- **Postgres** runs on a separate physical server to ensure data isolation and reliability.  
- Security is enforced using **perimeter firewalls** and **internal firewalls** between the web server and database.  
- **Hardware load balancers** distribute traffic across web servers for performance and redundancy.

**Key Points:**  
- Full control over hardware, software, and network configuration.  
- Ideal for organizations with strict compliance, security, or data residency requirements.  
- All responsibilities, including updates, backups, scaling, and security, fall on the organization.  
- Higher upfront costs and maintenance compared to cloud solutions.

---

This guide provides an overview of how a **React + Flask + Postgres** application can be deployed in different infrastructure environments, highlighting the differences in control, responsibility, and operational effort for each deployment model.


