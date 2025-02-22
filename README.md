## HashiCorp Vault Tutorial for Beginners What Why and How

- https://www.youtube.com/watch?v=klyAhaklGNU 

> Why was it created in the first place ? 

> What problems does it solve ?

> How does Vault solve the problem ? 

> Unique features how vault works ?

> Vault architecture 

#### Secrets Management Tools

> What are Secrets ?

- Information we use to **login or authorize** with various services 

```
Login ( User Name and Password )
Database Credentials
API Token ( To connect to a API )
``` 

Two types of users

- Human Users

```
As a engineers we need to login to Kubernetes Cluster ( Administer or Troubleshoot )
```
```
AS a engineer uses username/password to access AWS account 
```
```
Terraform (IaC) uses AWS IAM Role ( Key pair ) to provision infrastructure on AWS 
```

- System Users

```
CICD Pipeline ( To access the Cluster and Deploy to K8s cluster automatically ) 
```

Secretes between systems 

> If not properly secured, bad actors can misuse the those credentials and tokens

```
Web Application needs to access the Database ( Mysql, Postgres, MongoDB, ElasticSearch )
```
```
Social Media Login ( Facebook, Google, Twitter ) - API Keys
```
```
We could also be connecting to an external payment provider ( Stripe, Paypal ) 

- API Key to identify and authorize our application with Stripe
```

```
Configuring hundreds of servers with Ansible ( SSH Key Pairs for Remote Servers )
```

```
TLS Certificates for HTTPS Connection ( Certificate Authority ) - FrontEnd and Backend Communication 
```

```
TLS Certificates for Secure Communication within Kubernetes Cluster ( Istio mTLS ) - Generate Certificates for all pods in Kubernetes Cluster, in order to encrypt the communication between them 
```

> Why do we need to manage them ? 

- Where to keep **those Secrets** ?

- **How to make sure that** only the right people or system have access to them ?

#### Where Secrets are Stored ?

Local on the computer ( Not a secure place ) 

> Keys Pass Files shared around by team members 

```
AWS User Access Keys
SSH Keys to connect to remote server 
Kubeconfig file to connect to Kubernetes Cluster 
```

When a new junior engineer asks: I need a access to the Dev Database to test my code changes, where does I get those credentials ? 

```
Password File ( Passwords stored in a file )
Link to a Confluence Page ( List of Database credentials for each environment in plain text ) 
```

It is thought as a Secure Place 

> Reference secrets from pipeline **environment variables** 

> **Reference variable** in pipeline code instead of hardcoding credentials

- **Create env variables in GitLab CI/CD settings** for all services it needs to connect to

```
Docker Registry Credentials
Aws Credentials 
Kubernetes Cluster Credentials 
SSH Keys   
```

All Secrets that the Web Application needs to connect to Databases and API endpoints 

- External configuration files with Database credentials and API Tokens that get passed by CI/CD pipeline 

> Set and passed by CI/CD pipeline to the Docker Container

> Set as **OS environment variable the server** 

> **Hard coding credentials** in plain text in source code that gets to Git Repository or Commit History 

> Hard coding credentials in **Iac scripts**

- Ansible

- Puppet

- Terraform

#### Secret Sprawl 

Who has access to what secret ?

- In each **places** are the credentials stored ?

- **Who has access** to those places ?


Who has accessed or used what secrets ?

- **No detailed logs of access** and changes to secrets 

- It give us **very little audiability** 

- **No audit trail** of who accessed what secrets

> API was stolen 

- **Delete** old API key and **Create** a new one

- **Update** all the systems, where the API key is used

Generally, we want to **rotate keys regularly**

- **Manual Process*: Manually rotating secrets lead to human errors and inconsistent updates

- Ensuring all the components that rely on the secret are **updated simultaneously can be difficult** without automation a centralized system 

- Scalability Issue: AS number of secrets grow, managing secrets and rotation becomes unmanageable 

## Vault Centralized Secret Management Tool  

Examples of Secret Management Tools

- AWS Secrets Manager 

- Azure Key Vault

- GCP Secret Manager

#### Capabilities 

Purpose of such tools

> Offer **centralized platform** to manage all secrets, simplifying all the administration and access control

> Provide a **secure** storage solution, protecting them from unauthorized access and potential leaks 

Take all the secrets from:

```
Confluence Page 
Server Operating System 
Configuration Files
Engineers Local Computers 
```

And set in one Central Secure Place 

> Vault Secret Store

```
API Key A    DB Password    Stripe Key

    Certificate X    Admin SSH Key
```

#### Encryption at rest 

> Secrets are stored in a secure, **encrypted** format

> "At rest" refers to the processing of encrypting data when it is stored on disk or other persistent storage medium 

#### Encryption in transit

> Refers to encrypting data while it is being **transmitted over a network**

Fine-grained **access control** 

> Allow admins to define **who can access which secrets** under what conditions 

> Often integrating with identity and access management systems to enforce policies 

> Grant or forbid access to certain secrets and operations 

> Ensure engineers and apply **ONLY have access to the secrets they need** 

**Audit** and compliance 
 
> **Detailed logs of access** and changes to secrets

> Help organizations **meet regulatory compliance** requirements by tracking access to sensitive Information

#### Applications or Junior Devs may accidentally expose the secret 

If we have a Log Collector and Processor, those logs will be send to a Central Storage and then off to a Log Visualization tool

## Dynamic Secrets Feature 

In place off 

- Static Credentials ( Long-lived credentials ) / Login and Password

- Static API Keys ( Credentials that never expire)

#### Short-lived credentials with short expiration time

> Vault destroids the credentials when validity period expires

> **Create dynamiclly**, on demand

> So dynamic secrets do not exist when they are read, so there is **no risk of someone stealing them**

> Every client gets its own **unique** credentials

- We will know, which client was the vulnerable source

- This enable us to pinpoint and fix the issue

> Credential rotation needs to happen **only for that specif client**

> Prevent outage for all clients
> 

## Encryption as a Service ( Vault )

**Secrets** = Authentication Credentials, Encryption Keys 

- We also have regular data, which are not secrets but sensitive information 

> Personal Identifiable Information ( PII )

- Data that can be used to **identify** an individual 

```
Social Secure Number 
Passport Number
Driver's License Number
```

> Use Vault for **encrypting and decrypting your data** ( in transit and at rest ) 

> Vault handles the complexities of encryption key generation, storage and lifecycle management 

```
Vault Secret Store 
DB Credentials ( Encrypted )
Encryption Keys ( Encrypted )
```

#### Vault 

> **How** does it work technically ?

> **How** does it store the secrets ?

> **How** does it manage the secrets ?

> **How** does it provide dynamic secrets and encryption as a service ?

Vault Architecture

**Core**

> The Vault server is the **central component** that processes all the requests 

> **Manages the flow of requests** through the system, enforces ACLS and ensures audit logging is done

> Vault is a API Driven system that **all communication** between the clients and the Vault are done **through Vault API**

Vault comes with those various pluggable components allowing you to **integrate with external systems**

#### Secrets Engines 

> Components, which store, generate or encrypt data

> Each **secret engines handles different types of secrets** and operations 

Key/Value secrets engine 

> Generic Key-Value store used to store **arbitrary secrets** ( Static Secrets )

Database secrets engine

> Generates db credentials dynamically based on configured roles like Mysql, Oracle, PostgresSQL ( Dynamic Secrets )

- https://developer.hashicorp.com/vault/docs/secrets 

**RabbitMQ** Secrets Engine

> Generates user credentials dynamically based on configured permissions and virtual hosts 

**AWS** secret engine

> Generates AWS access credentials dynamically based on IAM Policies 

> IAM credentials are automatically revoked 

**PKI** secrete engine ( Public Key Infrastructure )

- Generate dynamic X.509 Certificates 

> Relives the burden of handling the complex certificates management

> Avoids have long-lived certificates ( Vault )

**SSH** secret engine

> Provides secure authentication and authorization for access to machine via the SSH protocol ( PEM file )

**Kubernetes** secret engine

> Generates k8s service account tokens 

Uses the **mechanism of those tools** to create temporary credentials for clients 

#### Secrets need to be physically stored in allocation 

**Storage Backend** 

> Storage backend provides a **durable data persistent layer**, where data is secured and available across server restarts

> Data like encrypted secrets, policies and configurations 

Standard Relational Database 

- Mysql

- Postgres

- HashiCorp Consul

- Cloud Managed Database Service

#### Authentication Methods or Authentication Backends 

> Auth methods perform authentication and are responsible for **assigining identity and a set of policies to a user**  

> Vault **delegates** the authentication to the relevant configured **external auth method**

> Vault validates client identity against **trusted third-party sources** 

**Login**

> These secrets need to be **securely accessible** with Vault 

Authenticate 

> Determine if they are who they say they are

> Vault first validates client **identity** 

```
I am ... from ...
I am EC2 Instance from AWS account with this ID 
I am Pod from Kubernetes Cluster with this Name
```

Authorization 

> Then, request is matched against Vault secure **policy**

> Rules defining which API endpoints a client has access to 

```
Can I please get credential for the Postgres Database ?
Can I please access the S# Bucket called this Name ?
Can I get the Strip API Key ?
```

**AWS** auth Method 

> Provides authentication mechanism for IAM principals and EC2 instances 

**Kubernetes** auth Method

> Can be used to authenticate with Vault using a K8s Service Account Token 

> Makes it easy to authenticate a Kubernetes Pod

Human requesting a secret

**LDAP** auth Method ( Active Directory Auth Plugging )

> Allows authentication using an exiting LDAP server and user/password credentials 

#### Client Token 

<- /secret/my-secret 

1. Vault authenticates client

2. **Token with a set of policies** is issued as response 

> Policies defines what secrets at which paths the client can access

> Token can be used for future requests, but is **short-lived** 

> Vault validates the Token and associated policy 

> Request is **routed to the secrets engine**

-> send key/value pair 

Enable any auth method you need

> Auth methods are **similar to secrets engines**

You can enable / disable using the CLI or the API 

```
vault auth enable userpass 
```

#### Audit Trail 

> Who has access to what ?

> When and how many times ?

**Visibility** of who is using what sensitive data

**Audit Devices**

How it works 

> Audit devices keep a **detailed log** of all requests and responses to vault

> Every interaction is captured 

```
Audit Broker ( Auditing Backend )

- Audit Device ( File, Socket, Syslog, HTTP )

Stream out request / response audit to it

```

**Audit trail** of who is doing what with the secrets in Vault

> The core logs requests / responses to the audit broker who **distributes** the request to **all configured audit devices**

**File** audit device

> Writes audit logs to a file 

**Syslog** 

> Writes to syslog - standard protocol used for sending logs to a centralized log server



















