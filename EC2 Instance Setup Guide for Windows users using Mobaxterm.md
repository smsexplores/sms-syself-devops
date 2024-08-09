# EC2 Instance Setup Guide

This guide will walk you through the steps to launch an EC2 instance and connect to it using MobaXterm.

## Step 1: Launch an EC2 Instance

1. **Log in to AWS Management Console:**
   - Go to the [AWS Management Console](https://aws.amazon.com).
   - Log in with your AWS credentials.

2. **Navigate to EC2 Dashboard:**
   - In the AWS Management Console, search for "EC2" in the search bar and select "EC2" to open the EC2 Dashboard.

3. **Launch a New Instance:**
   - Click the "Launch Instance" button.
   - Select an Amazon Machine Image (AMI). Choose an Ubuntu AMI (e.g., Ubuntu Server 20.04 LTS).
   - Choose an instance type. Select `t2.large`.
   - Configure instance details. You can leave most settings at their default values.
   - Add storage. The default storage is typically sufficient.
   - Add tags (optional).
   - Configure security group. Create a new security group or select an existing one. Ensure that SSH (port 22) is allowed.
   - Review and launch the instance. You will be prompted to select or create a key pair. Choose an existing key pair or create a new one and download the `.pem` file.

4. **Wait for the Instance to Start:**
   - After launching, wait for the instance to be in the "running" state. Note the public IP address of your instance.

## Step 2: Connect to Your EC2 Instance Using MobaXterm

1. **Open MobaXterm:**
   - Launch MobaXterm on your computer.

2. **Start a New SSH Session:**
   - Click on the "Session" icon in the top left corner.
   - Select "SSH" from the list of session types.

3. **Enter SSH Session Details:**
   - In the "Remote host" field, enter the public IP address of your EC2 instance.
   - Ensure that the "Specify username" checkbox is checked and enter `ubuntu` as the username (default for Ubuntu instances).

4. **Set Up SSH Key Authentication:**
   - Click on the "Advanced SSH settings" tab.
   - In the "Use private key" field, click on the folder icon and navigate to the location of your `.pem` file. Select the `.pem` file (e.g., `smskey1234.pem`).

5. **Start the SSH Session:**
   - Click "OK" to start the session.
   - MobaXterm will attempt to connect to your EC2 instance using the specified IP address and SSH key.

6. **Accept the SSH Key Fingerprint:**
   - The first time you connect, you might be prompted to accept the SSH key fingerprint. Type `yes` and press Enter.

7. **Successfully Connected:**
   - If all details are correct, you should now be connected to your EC2 instance, and you will see the command prompt for the `ubuntu` user.

## Troubleshooting Tips

- **Permission Issues with `.pem` File:** Ensure the `.pem` file has the correct permissions. Run `chmod 400 your-key-file.pem` to set the correct permissions.
- **Security Group Configuration:** Ensure your security group allows inbound SSH traffic on port 22 from your IP address.
- **Correct Username:** For Ubuntu instances, the default username is `ubuntu`.
- The default username can vary depending on the AMI you are using. Here are some common default usernames for different AMIs:
  1.Amazon Linux 2 or Amazon Linux AMI: ec2-user
  2.RHEL (Red Hat Enterprise Linux): ec2-user or root
  3.CentOS: centos or root
  4.Debian: admin or root
  5. SUSE Linux: ec2-user or root
  6. Ubuntu: ubuntu
  7. Fedora: ec2-user or fedora
When connecting via SSH, if you are unsure of the default username, check the documentation for the specific AMI you are using or try a combination of common usernames based on the operating system.

Following these steps, you should be able to launch and connect to your EC2 instance using MobaXterm. 
