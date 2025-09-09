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

## Setup Terraform
Setting up Terraform involves installing the Terraform software on your system. This enables you to create, modify, and manage infrastructure as code using Terraform configuration files.
1. Install the wget package using the command:
" sudo apt install wget -y "

2. Download and add the HashiCorp GPG key to the keyring:
" wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg "
" echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list"

3. Update the package index and install Terraform:
   " sudo apt update && sudo apt install terraform -y "
4. run the command “which terraform” which checks the location of the Terraform executable in your system's PATH
   <img width="714" height="102" alt="Screenshot 2025-09-09 at 19 56 58" src="https://github.com/user-attachments/assets/bd6d0aa1-abc0-4fcd-985f-dced3e0154f2" />
## Set up AWS CLI
1. Install the unzip package using the command:
   "sudo apt -y install unzip curl"
2. Download the AWS CLI installer package using curl:
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
3. Unzip the AWS CLI installer package:
unzip awscliv2.zip
4. Run the installer script with sudo privileges to install AWS CLI:
sudo ./aws/install

## Setup kubectl
Kubectl is a command-line tool used to interact with Kubernetes clusters. It allows users to deploy and manage applications, inspect and manage cluster resources, view logs,and execute debugging commands within a Kubernetes environment. Kubectl facilitates various operations such as creating, deleting, scaling, and updating Kubernetes resources.

1. Install the curl package using the command:
sudo apt install curl –y

2. Download the kubectl binary from the Kubernetes release page:
curl -LO https://dl.k8s.io/release/$(curl -L –s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
3. Set permissions and move the kubectl binary to the appropriate directory:
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Create an IAM Role for EC2
Why do we need an IAM role for EC2? → It is used by your EC2 instance to create an EKS cluster and manage s3 buckets. Applying for this IAM role gives authenticity to your EC2 to make changes on the AWS account.

Then Attach the IAM role to your EC2
Why do we need IAM Role →
Imagine you have a robot (EC2 instance) that does tasks for you in a big factory (AWS environment). Now, this robot needs to access different rooms (AWS services like S3, DynamoDB, etc.) to perform its tasks.

# Create an S3 Bucket
# Create Workspace & Clone Terraform Repo from GitHub

1. Create a directory named "super_mario" using the command:
mkdir super_mario
2. Navigate into the "super_mario" directory:
cd super_mario
3. git clone https://github.com/k21academyuk/Deployment-of-super-Mario-on-Kubernetes-using-terraform.git 
4. Move into the cloned repository directory:
cd Deployment-of-super-Mario-on-Kubernetes-using-terraform/
5. Move into the "EKS-TF" directory:
cd EKS-TF

# Initialize Terraform
1. Run terraform init to initialize the directory and download the provider plugins.
terraform init
<img width="741" height="533" alt="Screenshot 2025-09-09 at 20 58 18" src="https://github.com/user-attachments/assets/82a80a2b-3aac-4134-b951-abe45c1a596c" />

2. Validating the plan
Once the terraform directory is initialized, we will create an execution plan and will validate if it is correct or if there are any errors in this.

Validate the code.
terraform validate

<img width="730" height="231" alt="Screenshot 2025-09-09 at 21 05 04" src="https://github.com/user-attachments/assets/8d24561e-a9eb-4bd8-ac9e-d966025f900c" />

3. Run the terraform plan to validate the code and check if it’s valid or not. It will show all the changes that are going to take place when we will execute this plan
terraform plan


<img width="1470" height="802" alt="Screenshot 2025-09-09 at 21 08 49" src="https://github.com/user-attachments/assets/cd1be670-f92f-4d2a-8b77-a7556650b49f" /><img width="1470" height="800" alt="Screenshot 2025-09-09 at 21 09 17" src="https://github.com/user-attachments/assets/597b15bb-392f-4566-b2d9-384322b9c57b" /><img width="1470" height="801" alt="Screenshot 2025-09-09 at 21 09 37" src="https://github.com/user-attachments/assets/2f69d04a-3fe3-40f4-bb57-78ab049f0039" /><img width="1470" height="781" alt="Screenshot 2025-09-09 at 21 09 58" src="https://github.com/user-attachments/assets/7c1a1f93-ca13-4194-86fd-c27c512be80c" /><img width="1470" height="319" alt="Screenshot 2025-09-09 at 21 10 25" src="https://github.com/user-attachments/assets/7a466dcb-b337-4484-9137-5023b214fbe3" />


4. Execute the plan
Once the execution plan is validated, we can now apply or execute the plan to create a resource group.
Run terraform apply to execute the code. Type yes when prompted or you can use "terraform apply --auto-approve" and it will approve automatically.
"terraform apply -auto-approve"

<img width="1470" height="772" alt="Screenshot 2025-09-09 at 21 17 20" src="https://github.com/user-attachments/assets/22c23e91-2994-427d-a507-b7579122785f" />

<img width="1470" height="493" alt="Screenshot 2025-09-09 at 21 27 15" src="https://github.com/user-attachments/assets/7ab331dd-ceff-4367-b7d3-207d080bce6a" />
<img width="900" height="410" alt="Screenshot 2025-09-09 at 21 29 11" src="https://github.com/user-attachments/assets/ae94ed94-23c9-4f89-b0ed-e7a04d229cfa" />

# Creation of Deployment and Service for EKS
1.  cd ..
2.  Create the deployment.
kubectl apply -f deployment.yaml
deployment.yaml file is like a set of instructions that tells a computer system, "Hey, here's how you should run and manage a particular application ". It provides the necessary information for a computer system to deploy and manage a specific software application. It includes details like what the application is, how many copies of it should run, and other settings that help the system understand how to keep the application up and running smoothly.
3. Now create the service.
kubectl apply -f service.yaml

4. Run the command `kubectl get all` in your terminal or command prompt. This command retrieves information about all resources of type "all" in your Kubernetes cluster, including pods, services, deployments, and more.
kubectl get all

<img width="1470" height="391" alt="Screenshot 2025-09-09 at 21 42 12" src="https://github.com/user-attachments/assets/fe6a6750-d284-4a6c-bbe5-42c6179d1d16" />

5. Now Run the following command to get the load balancer ingress.
This command tells all the details of your application
kubectl describe service mario-service
<img width="1470" height="809" alt="Screenshot 2025-09-09 at 21 44 30" src="https://github.com/user-attachments/assets/9638de6e-a5e1-4e6c-8544-9ed2864f854d" />




# Summary of Project Documentation: "Deployment of a Gaming Application on Kubernetes using Terraform"

This documentation outlines a robust method for deploying a gaming application, specifically Super Mario, onto an Amazon Elastic Kubernetes Service (EKS) cluster. The project leverages Terraform for defining and provisioning the entire underlying AWS infrastructure, including the EKS cluster, networking components, and storage. Once the infrastructure is established, Kubernetes takes over to manage the game application's deployment, scaling, and orchestration, ensuring high availability and performance. The documentation provides a detailed, step-by-step guide on setting up the necessary tools (Docker, Terraform, AWS CLI, Kubectl) on an EC2 instance, configuring an IAM role for secure AWS access, creating an S3 bucket, and then proceeding with the Terraform and Kubernetes deployment commands. It emphasizes the benefits of this approach, such as automated infrastructure, scalability, resilience, and operational efficiency.
# Real-Time Use Case

Imagine a rapidly growing indie game studio launching a new multiplayer online game. They anticipate fluctuating player counts, with significant spikes during promotional events or new content releases.

   - Initial Launch: Using this project's methodology, the studio can quickly provision their entire game infrastructure on EKS with a few terraform apply commands. This eliminates manual setup and ensures consistency.

   - Scaling During Peak Hours: When a new update attracts thousands of players, Kubernetes automatically scales the game's pods to handle the increased load. This prevents server crashes and maintains a smooth gaming experience without manual intervention from the studio's operations team.

   - Global Expansion: As the game gains popularity worldwide, the studio can easily replicate this entire infrastructure in different AWS regions using Terraform. This provides low-latency access for players globally.

   - Patching and Updates: New game patches can be deployed seamlessly using Kubernetes' rolling updates, ensuring zero downtime for players. The declarative nature of the deployment.yaml and service.yaml files makes updates consistent and predictable.

 - Cost Optimization: Kubernetes can be configured to scale down resources during off-peak hours, optimizing cloud spending without sacrificing performance when demand is high.

This approach allows the gaming studio to focus on game development rather than infrastructure management, providing a highly scalable and resilient platform for their application.

# Benefits

   - Automated and Repeatable Infrastructure Provisioning: Terraform codifies the entire infrastructure, making deployments consistent, repeatable, and less prone to human error. This is crucial for disaster recovery and setting up new environments.

   - Enhanced Scalability and Resilience: Kubernetes automatically manages the scaling of game instances based on demand, ensuring the game remains responsive and available even under heavy load. Its self-healing capabilities improve application resilience.

   - Operational Efficiency: The combination of IaC with Terraform and container orchestration with Kubernetes significantly streamlines deployment pipelines, reduces manual operational tasks, and accelerates the release cycle for game updates and patches.

   - Cloud Agnostic (Terraform) and Vendor Neutral (Kubernetes): While this project uses EKS, Terraform supports multiple cloud providers, offering flexibility for future migration or multi-cloud strategies. Kubernetes itself is an open-source standard, preventing vendor lock-in at the application layer.

   - Version Control and Collaboration: Infrastructure and application configurations are stored as code in Git, enabling version control, collaboration among teams, and a clear audit trail of all changes.

   - Cost Optimization: Dynamic scaling capabilities of Kubernetes, combined with Terraform's ability to precisely define resources, help in optimizing cloud resource utilization and reducing unnecessary expenditures.

   - Simplified Management of Complex Systems: Breaking down the game into containerized microservices managed by Kubernetes simplifies the complexity of running a large-scale distributed application.

   - Faster Iteration and Development Cycles: Developers can quickly deploy and test new features or fixes in environments that closely mirror production, leading to faster development and iteration cycles.















