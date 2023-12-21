Hey folks, remember the thrill of 90's gaming? Let's step back in time and relive those exciting moments! With the game deployed on Kubernetes, it's time to dive into the nostalgic world of Mario. Grab your controllers, it's game time!

Super Mario is a classic game loved by many. In this guide, we'll explore how to deploy a Super Mario game on Amazon's Elastic Kubernetes Service (EKS). Utilizing Kubernetes, we can orchestrate the game's deployment on AWS EKS, allowing for scalability, reliability, and easy management.

Prerequisites:
An Ubuntu Instance

IAM role

Terraform should be installed on instance

AWS CLI and KUBECTL on Instance

LET'S DEPLOY
STEP 1: Launch Ubuntu instance
Sign in to AWS Console: Log in to your AWS Management Console.

Navigate to EC2 Dashboard: Go to the EC2 Dashboard by selecting "Services" in the top menu and then choosing "EC2" under the Compute section.

Launch Instance: Click on the "Launch Instance" button to start the instance creation process.

Choose an Amazon Machine Image (AMI): Select an appropriate AMI for your instance. For example, you can choose Ubuntu image.

Choose an Instance Type: In the "Choose Instance Type" step, select t2.micro as your instance type. Proceed by clicking "Next: Configure Instance Details."

Configure Instance Details:

For "Number of Instances," set it to 1 (unless you need multiple instances).

Configure additional settings like network, subnets, IAM role, etc., if necessary.

For "Storage," click "Add New Volume" and set the size to 8GB (or modify the existing storage to 8GB).

Click "Next: Add Tags" when you're done.

Add Tags (Optional): Add any desired tags to your instance. This step is optional, but it helps in organizing instances.

Configure Security Group:

Choose an existing security group or create a new one.

Ensure the security group has the necessary inbound/outbound rules to allow access as required.

Review and Launch: Review the configuration details. Ensure everything is set as desired.

Select Key Pair:

Select "Choose an existing key pair" and choose the key pair from the dropdown.

Acknowledge that you have access to the selected private key file.

Click "Launch Instances" to create the instance.

Access the EC2 Instance: Once the instance is launched, you can access it using the key pair and the instance's public IP or DNS.

Ensure you have necessary permissions and follow best practices while configuring security groups and key pairs to maintain security for your EC2 instance.

STEP 2: Create IAM role
Search for IAM in the search bar of AWS and click on roles.



Click on Create Role



Select entity type as AWS service

Use case as EC2 and click on Next.



For permission policy select Administrator Access (Just for learning purpose), click Next.



Provide a Name for Role and click on Create role.



Role is created.



Now Attach this role to Ec2 instance that we created earlier, so we can provision cluster from that instance.

Go to EC2 Dashboard and select the instance.

Click on Actions --> Security --> Modify IAM role.



Select the Role that created earlier and click on Update IAM role.



Connect the instance to Mobaxtreme or Putty

STEP 3: Cluster provision
Now clone this Repo.


COPY

COPY
git clone https://github.com/Aj7Ay/k8s-mario.git


change directory


COPY

COPY
cd k8s-mario
Provide the executable permission to script.sh file, and run it.


COPY

COPY
sudo chmod +x script.sh
./script.sh


This script will install Terraform, AWS cli, Kubectl, Docker.

Check versions


COPY

COPY
docker --version
aws --version
kubectl version --client
terraform --version


Now change directory into the EKS-TF

Run Terraform init

NOTE: Don’t forgot to change the s3 bucket name in the backend.tf file


COPY

COPY
cd EKS-TF
terraform init


Now run terraform validate and terraform plan


COPY

COPY
terraform validate
terraform plan


Now Run terraform apply to provision cluster.


COPY

COPY
terraform apply --auto-approve


Completed in 10mins



Update the Kubernetes configuration

Make sure change your desired region


COPY

COPY
aws eks update-kubeconfig --name EKS_CLOUD --region ap-south-1


Now change directory back to k8s-mario


COPY

COPY
cd ..
Let’s apply the deployment and service

Deployment


COPY

COPY
kubectl apply -f deployment.yaml
#to check the deployment 
kubectl get all


Now let’s apply the service

Service


COPY

COPY
kubectl apply -f service.yaml
kubectl get all


Now let’s describe the service and copy the LoadBalancer Ingress


COPY

COPY
kubectl describe service mario-service


Paste the ingress link in a browser and you will see the Mario game.

Let’s Go back to 1985 and play the game like children.



This is official image by MR CLOUD BOOK

You can check in Docker-hub as well sevenajay/mario:latest



Destruction :
Let's remove the service and deployment first


COPY

COPY
kubectl get all
kubectl delete service mario-service
kubectl delete deployment mario-deployment
Let’s Destroy the cluster


COPY

COPY
terraform destroy --auto-approve


After 10mins Resources that are provisioned will be removed.



Thank you for joining this nostalgic journey to the 90s! We hope you enjoyed rekindling your love for gaming with the deployment of the iconic Mario game on Kubernetes. Embracing the past while exploring new technologies is a true testament to the timeless allure of classic games. Until next time, keep gaming and reliving those fantastic memories! 👾🎮.
