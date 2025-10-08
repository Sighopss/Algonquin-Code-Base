# Cloud Resource Descriptions and Comparison Table

| #  | Description | AWS (Service Name) | Azure (Service Name) | Google Cloud (Service Name) |
|----|-------------|--------------------|----------------------|----------------------------|
| 1  | A compute service that provides scalable virtual machines for running applications. | EC2 | Virtual Machines | Compute Engine |
| 2  | An object storage service used to store and retrieve data, commonly used for backups and static website content. | S3 | Blob Storage | Cloud Storage |
| 3  | A managed relational database service that supports multiple database engines like MySQL, PostgreSQL, and SQL Server. | RDS | SQL Database | Cloud SQL |
| 4  | A serverless compute service that allows you to run code in response to events without provisioning or managing servers. | Lambda | Functions | Cloud Functions |
| 5  | A virtual private network service that allows you to create isolated networks within the cloud provider's infrastructure. | VPC | Virtual Network | VPC |
| 6  | A content delivery network (CDN) service that delivers data, videos, applications, and APIs to customers around the world with low latency. | CloudFront | CDN | Cloud CDN |
| 7  | A managed NoSQL database service designed for low-latency, high-scale applications. | DynamoDB | Cosmos DB | Firestore |
| 8  | A block storage service for use with virtual machines, offering persistent storage for data. | EBS | Managed Disks | Persistent Disk |
| 9  | A managed container orchestration service based on Kubernetes. | EKS | AKS | GKE |
| 10 | A service for managing user access and encryption keys to secure cloud resources. | IAM | Microsoft Entra ID | IAM |
| 11 | A platform that automates application deployment and scaling without needing to manage infrastructure. | Elastic Beanstalk | App Service | App Engine |
| 12 | A service that provides monitoring and logging of applications and infrastructure, offering insights into resource usage and performance. | CloudWatch | Monitor | Cloud Monitoring |
| 13 | A domain name system (DNS) service that routes traffic globally and translates domain names to IP addresses. | Route 53 | DNS | Cloud DNS |
| 14 | A load balancing service that distributes incoming network traffic across multiple targets, improving application availability. | Elastic Load Balancing | Load Balancer | Load Balancing |
| 15 | A service that automatically scales your cloud infrastructure based on demand, ensuring resources are available as needed. | Auto Scaling | Virtual Machine Scale Sets | Autoscaler |
| 16 | A message queuing service that enables applications to send and receive messages between different components. | SQS | Service Bus | Pub/Sub |
| 17 | A managed real-time data streaming service that collects and processes large amounts of data from various sources. | Kinesis | Event Hubs | Pub/Sub |
| 18 | A fully managed, highly scalable data warehouse service optimized for analytics and large-scale queries. | Redshift | Synapse Analytics | BigQuery |
| 19 | A service that automates the execution of workflows and allows the integration of different cloud services in a sequence of steps. | Step Functions | Logic Apps | Cloud Workflows |
| 20 | A service that integrates multiple data sources and enables data migration, transformation, and movement across platforms. | Glue | Data Factory | Dataflow |
| 21 | A data catalog and governance service that helps manage metadata across your data estate, supporting compliance and security. | Glue Data Catalog | Purview | Data Catalog |
| 22 | A set of machine learning and AI services that provide pre-built models, APIs, and tools for developers to easily implement AI in their apps. | SageMaker | Cognitive Services | Vertex AI |
| 23 | A service that allows you to define and deploy infrastructure using code, automating the management of cloud resources. | CloudFormation | ARM Templates | Deployment Manager |
| 24 | A fully managed CI/CD service that automates the building, testing, and deployment of applications to production environments. | CodePipeline | DevOps | Cloud Build |
| 25 | A desktop as a service (DaaS) offering that allows you to deploy virtual desktops in the cloud and access them remotely. | WorkSpaces | Virtual Desktop | Chrome Remote Desktop |
| 26 | A backup and disaster recovery service that helps to protect your data by creating backups and replicas of your cloud resources. | Backup | Backup | Backup and DR Service |
| 27 | A service designed for big data analytics, allowing organizations to store, process, and analyze large datasets in real time. | EMR | HDInsight | Dataproc |
| 28 | A file storage service for storing and sharing files with users, typically used in shared file systems across applications. | EFS | Files | Filestore |
| 29 | A service that helps you transcode, process, and stream media content such as video and audio. | Elemental MediaConvert | Media Services | Transcoder API |
| 30 | A real-time communication service used for sending notifications, emails, and text messages to users and devices. | SNS | Notification Hubs | Firebase Cloud Messaging |

# Comparison Of Cloud Services Across AWS, Azure, and Google Cloud

## Overview
AWS, Azure and Google Cloud are the leading cloud service providers in today’s market. Each platform offers a similar range of core cloud computing services but they differ in naming conventions, capabilities and integrations with their respective ecosystems.  
The report highlights the key similarities and differences among equivalent services from these providers, identifies unique features of specific offerings and explains how naming conventions vary across platforms.

## Key Similarities and Differences

### Compute Services
All three providers offer scalable virtual machines for running applications.  
AWS, Azure and Google provide similar functionality in provisioning VMs with flexible OS options and configs.  
AWS leads with the widest instance variety and spot pricing flexibility.  
Azure integrates deeply with Microsoft tools like Windows Server and Active Directory.  
GCP stands out for its sustained use discounts and live migration of VMs without downtimes.

### Storage Services
Each provider offers reliable object, block and file storage solutions:  
AWS, Azure and Google Cloud are equivalent object storage services for backups and static data.  
AWS pioneered the model and has the most mature ecosystem of them all.  
Azure’s storage easily integrates with Microsoft 365 and enterprise systems.  
GCP emphasizes performance and automatic multi-regional redundancy.

### Databases
For relational databases:  
RDS (AWS), Azure SQL Database and Cloud SQL by GCP deliver managed database engines like MySQL and PostgreSQL.  
AWS RDS supports the broadest selection of engines while Azure SQL Database offers the deepest integration with Microsoft SQL Server tools.  
GCP Cloud SQL excels in automation and integration with BigQuery for analytics.  

For NoSQL:  
DynamoDB, Cosmos DB, and Firestore are the offerings.  
Cosmos DB supports multiple APIs making it more versatile than DynamoDB and Firestore.

### Networking
VPC (AWS & GCP) and Virtual Network (Azure) serve similar purposes by allowing private, isolated networks.  
Azure’s Virtual Network integrates seamlessly with on-premises systems via ExpressRoute, while AWS emphasizes extensive control and subnet configuration flexibility.

### Serverless and Containers
AWS Lambda, Azure Functions and Google Cloud Functions all allow developers to run event-driven code without managing servers.  
AWS Lambda has the most mature ecosystem and triggers, while GCP integrates tightly with Firebase and Pub/Sub.  
For container orchestration, EKS, AKS and GKE all rely on Kubernetes—though GKE is considered the most fully managed and developer-friendly.

### Analytics and Machine Learning
AWS Redshift, Azure Synapse Analytics and Google BigQuery are managed data warehouse services.  
BigQuery stands out for its serverless model and real-time analytics whereas Redshift offers better control over infrastructure.  
In AI/ML, AWS SageMaker, Azure Cognitive Services, and Google Vertex AI offer similar capabilities. SageMaker focuses on end-to-end ML model development, Cognitive Services provides pre-built APIs, and Vertex AI integrates ML pipelines with data analytics.

## Unique Features and Capabilities
AWS is renowned for its global infrastructure, developer-focused features, and wide range of services. Highly configurable tools like CodePipeline for CI/CD automation and CloudFormation for infrastructure as code are part of its ecosystem.  

Azure makes use of Microsoft's business presence. It is perfect for hybrid cloud deployments because its services, like App Service and Entra ID (formerly Azure AD), seamlessly integrate with Windows environments and Office 365.  

Google Cloud prioritizes AI innovation, data analytics, and simplicity. Large-scale data processing and machine learning are best served by tools like BigQuery, Vertex AI, and Dataflow. Deep open-source connections (Kubernetes, for example, was developed at Google) are another way that GCP sets itself apart.

## Naming Conventions Across Providers
The naming and branding styles of the three providers differ noticeably from one another:  

- Short, technical, and useful names are frequently used by AWS (e.g., EC2, S3, IAM).  
- Azure prefers names that are human-readable, descriptive, and consistent with enterprise jargon (e.g., Virtual Machines, Blob Storage, App Service).  
- Google Cloud frequently prefixes services with “Cloud” or descriptive terms (e.g., Cloud Storage, Cloud Functions, BigQuery) in an effort to combine simplicity and clarity.  

This represents the target market for each provider: developers and startups are drawn to AWS, enterprise IT professionals are drawn to Azure, and data engineers and artificial intelligence experts are drawn to GCP.

