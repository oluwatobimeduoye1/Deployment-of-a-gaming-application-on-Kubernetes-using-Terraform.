# Deployment-of-a-gaming-application-on-Kubernetes-using-Terraform.
This project details the deployment of a game within a Kubernetes cluster, utilizing Terraform for comprehensive infrastructure provisioning. Terraform is responsible for defining and establishing all requisite infrastructure components, including Kubernetes clusters, associated networking resources, and persistent storage solutions. Upon infrastructure initialization, Kubernetes assumes control of the game application's deployment, scaling, and orchestration. This integrated approach guarantees enhanced scalability, bolstered reliability, and optimized management of the game's application infrastructure.

Key benefits include:
- Automated Infrastructure Provisioning: Terraform defines and manages the cloud infrastructure, enabling consistent and repeatable deployments.
- Scalability and Resilience: Kubernetes dynamically scales game instances based on demand, ensuring a smooth user experience even during peak traffic.
- Operational Efficiency: The combination of Terraform and Kubernetes streamlines deployment pipelines and reduces manual intervention, leading to faster iterations and reduced operational overhead.

This approach provides a robust and agile solution for deploying and managing modern gaming applications at scale

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/3d0bb6e5-98b3-4f89-9893-fb9dab1e3e1a" />

# OVERVIEW

### What is Terraform?
Terraform is an open-source Infrastructure as Code (IaC) tool developed by HashiCorp. It allows users to define and provision infrastructure in a declarative configuration language. With Terraform, you can manage and automate the deployment of infrastructure resources across various cloud providers, on-premises environments, and even third-party services.

Key features of Terraform include:
- Declarative Configuration: Infrastructure is defined in human-readable configuration files, specifying the desired state of the infrastructure.
- Multi-Cloud Support: Terraform is cloud-agnostic and supports multiple cloud providers, including Amazon Web Services (AWS), Microsoft Azure, Google Cloud Platform (GCP), and others.
- Resource Graph: Terraform builds a dependency graph of resources, enabling efficient provisioning and parallel execution of infrastructure changes.
- State Management: Terraform tracks the current state of infrastructure, allowing it to plan and apply only the necessary changes to achieve the desired state

### What is EKS?
Amazon Elastic Kubernetes Service (EKS) is a managed Kubernetes service provided by Amazon Web Services (AWS). It simplifies the process of deploying, managing, and scaling Kubernetes clusters in the AWS cloud environment. With EKS, users can run containerized applications on Kubernetes without installing or operating the Kubernetes control plane themselves. 
EKS integrates with other AWS services for security, networking, monitoring, and logging, offering a reliable and scalable platform for running Kubernetes workloads.

### What is Docker?
Docker is a platform and set of tools designed to make it easier to create, deploy, and run applications by using containers. 
Containers allow developers to package up an application with all the parts it needs, such as libraries and dependencies, and ship it as one package. This ensures that the application will run in any environment.

### What is KubeCTL?
Kubectl is a command-line tool used to interact with Kubernetes clusters. It allows users to deploy and manage applications, inspect and manage cluster resources, view logs, and execute debugging commands within a Kubernetes environment. 
Kubectl facilitates various operations such as creating, deleting, scaling, and updating Kubernetes resources.


# Launch & Configure EC2 Instance
we begin by creating an EC2 instance and configuring it so we can install Terraform and Docker in our next step.
1. Launch an Ubuntu server
2. Connect to Ubuntu server
3. Run the following commands 
“sudo su” to provide root user permission.
“apt update -y” to update the package lists for upgrades and new packages in Ubuntu EC2 Instance

# Set up Docker, Terraform, AWS CLI & Kubectl
## Setup Docker
1. Install Docker by executing the following command:
   " apt install docker.io "
   <img width="720" height="421" alt="Screenshot 2025-09-09 at 19 32 53" src="https://github.com/user-attachments/assets/798c765a-4f3d-4b49-9148-d738c978b362" />
2. Add your user to the Docker group using the command:
   "usermod -aG docker $USER"
3. After adding your user to the Docker group, activate the changes by running the command:
  " newgrp docker "
4. run the command " which docker ", it checks the location of the Docker executable in your system's PATH.

 <img width="724" height="272" alt="Screenshot 2025-09-09 at 19 40 52" src="https://github.com/user-attachments/assets/3dbff677-22dd-4209-8c35-3d8de9f7efbc" />
Docker successfully Installed!!!!











