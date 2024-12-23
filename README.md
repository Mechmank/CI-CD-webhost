**CI-CD Webhost**

This project demonstrates how to host a website on an Ubuntu server under a CI/CD pipeline using GitHub Actions. It automates the process of deploying web files (e.g., HTML, CSS, etc.) to an EC2 instance hosted on AWS.

Table of Contents
Project Overview
Prerequisites
Setup
AWS EC2 Instance Setup
GitHub Self-Hosted Runner Configuration
CI/CD Workflow
Usage
License
Acknowledgments
Project Overview
This project automates the deployment of a static website (HTML, CSS, etc.) to an Ubuntu server hosted on AWS EC2. The website is deployed every time there is a change in the repository, using GitHub Actions as the CI/CD tool. The CI/CD pipeline is configured with a self-hosted runner to securely deploy web files to the EC2 server.

Prerequisites
Before setting up the project, ensure the following tools and services are available:

AWS Account: To create and manage EC2 instances.
Ubuntu EC2 Instance: Running Ubuntu server to host the website.
GitHub Repository: This project should be hosted on GitHub.
GitHub Actions: Used for automating the CI/CD pipeline.
Self-hosted GitHub Runner: Installed on your Ubuntu EC2 instance.
Domain Name (Optional): For accessing the website through a custom domain (optional).
Setup
AWS EC2 Instance Setup
Create an EC2 Instance:

Log in to your AWS console and create an EC2 instance with the desired configuration (Ubuntu server recommended).
Ensure the security group allows HTTP (port 80) and SSH (port 22) access.
Install Required Software:

SSH into your EC2 instance and install a web server (e.g., Apache, Nginx).
bash
Copy code
sudo apt update
sudo apt install apache2
Obtain Server IP:

Note down your EC2 instance's public IP address. This will be used to configure the deployment.
GitHub Self-Hosted Runner Configuration
Set Up GitHub Self-Hosted Runner:

On your EC2 instance, download and configure the GitHub Actions self-hosted runner. Follow these instructions from the GitHub documentation.
Install Dependencies:

Ensure docker and any other required dependencies are installed on your EC2 instance.
Register the Runner:

On your EC2 instance, run the following commands to register the self-hosted runner with your GitHub repository:
bash
Copy code
# Example GitHub runner setup commands:
./config.sh --url https://github.com/username/repository --token YOUR_TOKEN
Start the Runner:

Start the runner to allow GitHub Actions to trigger workflows.
bash
Copy code
./run.sh
CI/CD Workflow
Create GitHub Actions Workflow:

In your GitHub repository, create a .yml file under the .github/workflows directory. This file will define the CI/CD pipeline.
Example workflow file:

yaml
Copy code
name: Deploy Website to EC2

on:
  push:
    branches:
      - main  # Trigger deployment when changes are pushed to the main branch

jobs:
  deploy:
    runs-on: self-hosted  # Use self-hosted runner

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Deploy to EC2 Server
      run: |
        ssh -i /path/to/private-key user@your-ec2-ip "cd /var/www/html && git pull origin main"
Configure SSH Keys:

Ensure you configure SSH key access from GitHub Actions to your EC2 instance for secure communication.
Automate Deployment:

After setting up the workflow, every time you push changes to the main branch, GitHub Actions will trigger the workflow, and your website will be deployed to the EC2 instance.
Usage
Once everything is set up, every change you push to the main branch will automatically trigger the CI/CD pipeline. The pipeline will:

Pull the latest changes from the GitHub repository.
Deploy the files to the server.
Update the website on the EC2 instance.
Accessing the Website
Once the CI/CD pipeline completes, you can visit your website using the public IP address of your EC2 instance or a custom domain (if configured).

Example:

arduino
Copy code
http://<your-ec2-ip>
License
This project is licensed under the MIT License - see the LICENSE file for details.

Acknowledgments
GitHub Actions Documentation
AWS EC2 Documentation
Special thanks to the contributors and community who helped improve this process.
