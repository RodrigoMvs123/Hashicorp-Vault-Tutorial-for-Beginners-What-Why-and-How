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

> Who has access to what secret ?

- In each **places** are the credentials stored ?

- **Who has access** to those places ?





