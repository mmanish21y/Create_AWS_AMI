# Create AWS AMI with Custom Package and NGINX Installation
# This guide outlines the necessary steps to create an AWS AMI with the following setup:

## Custom Package Installation

NGINX Web Server Installation
Prerequisites:

AWS Account: Ensure you have access to an AWS account.
AWS CLI: AWS Command Line Interface (CLI) installed and configured.
IAM Permissions: Ensure that your AWS user/role has permissions to create AMIs, EC2 instances, and other relevant resources.
SSH Key: Have a valid SSH key pair to access the EC2 instance.

## Step-by-Step Guide
### 1. Launch a Base EC2 Instance
Choose an AMI: Select a base AMI that will serve as the foundation for the new image. A common choice is Amazon Linux 2 or Ubuntu.
Instance Type: Choose an instance type, e.g., t2.micro, depending on your requirements.
Network Configuration: Make sure your instance is in the correct VPC and subnet. Ensure you have proper security group rules that allow SSH (port 22) and HTTP (port 80).
SSH Key Pair: Select an existing key pair or create a new one to SSH into the instance.
After the instance is launched, connect to it via SSH.

### 2. Update the Instance and Install Dependencies
SSH into the EC2 instance and run the following commands to update the system and install necessary dependencies:

bash
Copy code
# Update the package list
sudo yum update -y   # For Amazon Linux / CentOS / RHEL
sudo apt-get update -y   # For Ubuntu

# Install necessary tools (optional but recommended)
sudo yum install -y wget curl vim git   # Amazon Linux / CentOS / RHEL
sudo apt-get install -y wget curl vim git   # Ubuntu
### 3. Install Custom Package(s)
Install your custom packages. For example, if you are installing a custom package, you can use yum or apt-get depending on your instance's OS. Here's an example for a package installation:
bash
Copy code
# Example: Install a custom package
sudo yum install -y <custom-package>   # For Amazon Linux / CentOS / RHEL
sudo apt-get install -y <custom-package>   # For Ubuntu
If the custom package needs to be installed from a source or third-party repository, follow the specific installation instructions provided by the vendor or package source.

### 4. Install NGINX Web Server
Install NGINX on your EC2 instance by following the steps below:

bash
Copy code
# Install NGINX (Amazon Linux / CentOS / RHEL)
sudo amazon-linux-extras install nginx1 -y
sudo service nginx start
sudo service nginx enable

# For Ubuntu
sudo apt-get install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx
Verify that NGINX is working by navigating to your EC2 instance's public IP address in a browser. You should see the default NGINX page.
bash
Copy code
curl http://<EC2-public-IP>
### 5. Configure NGINX (Optional)
If required, update the NGINX configuration. For example, you may want to configure server blocks, change the default page, or adjust other settings.
Configuration files are usually located at /etc/nginx/nginx.conf or /etc/nginx/conf.d/.
Example:

bash
Copy code
sudo vim /etc/nginx/nginx.conf
sudo systemctl restart nginx

### 6. Create an AMI from the Configured Instance
Once you have installed your custom package(s) and set up NGINX, it's time to create an AMI (Amazon Machine Image) from the configured instance.
Steps:

### Open the Amazon EC2 Console.
In the left-hand menu, select Instances.
Find the instance you configured, right-click, and select Create Image.
In the Create Image dialog:
Image Name: Enter a name for the AMI, e.g., custom-ami-with-nginx.
Image Description: Add an optional description.
No Reboot: Optionally, uncheck "No Reboot" to allow the instance to reboot before creating the image (recommended).
Click Create Image.
AWS will begin creating the AMI. The process may take a few minutes. You can monitor the status in the AMIs section of the EC2 dashboard.

7. Test the Created AMI
Launch a new EC2 instance from the AMI you just created to ensure that your custom package and NGINX installation were successful.
Once the instance is running, connect to it via SSH and test whether the custom package is installed and NGINX is working properly.
You can access the NGINX web server by navigating to the public IP of the new instance in a web browser.
8. Clean Up (Optional)
If you no longer need the instance used to create the AMI, you can terminate it to avoid ongoing charges.
To delete the AMI, go to the AMIs section in the EC2 dashboard, select the image, and choose Deregister.
Additional Notes:
Security Groups: Ensure that the security group associated with the instance allows inbound traffic on the ports your custom package and NGINX require (e.g., HTTP - port 80).
Elastic IP: For persistent access, consider associating an Elastic IP with the EC2 instance before testing.
Conclusion:
By following these steps, you will have successfully created an AWS AMI that includes a custom package installation and NGINX setup. You can now use this AMI to launch new EC2 instances with the same configuration, saving you time on future setups.

