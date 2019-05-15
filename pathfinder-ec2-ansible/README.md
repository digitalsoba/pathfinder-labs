# Pathfinder EC2 and Ansible
This lab will go through the process of creating an EC2 instance, running an ansible playbook, and creating an image from that provisioned machine. 

## Creating an EC2 Instance
Login to your AWS Console. Navigate to **Services** --> **Compute** --> **EC2**. 

Click **Launch Instance** and chose `Ubuntu Server 18.04 LTS (HVM)` as the AMI

Select `t.2 micro` as your instance type 

On step 3, **Configure Instance Details** we'll use the default settings. Click **Next: Add Storage**, again we'll use the default settings so we can click **Next: Add Tags**

On the Tags page, we will add a name to our instance. Click **Add Tag**. For the key, type: `Name` and the Value type what you want to name the instance. Next click **Configure Security Group**

We're going to create our own security group for this lab. Change the **Security group name** to `pathfinder-security-group`. It's also useful to include a description. For this lab, we will open up port 22 and 80 (ssh and HTTP). By default, the ssh rule is already included. We need to add **Add Rule** Another row should populate, we can click type and select **HTTP**. It should refill the other fields. 

Now it's time to **Review and Launch** our instance. When ready, click **Launch** 

A Pop-up should show asking to create or use an existing key pair. Let's create a new key pair; give it a name. Also, download the key pair, it should be a `.pem` file. Make sure you don't lose this as it's the only way you can access your EC2 instance! Now we can navigate to our EC2 instance on the AWS console. Let's wait for it to boot up. Once the Instance State is `running` the EC2 is ready for the next part of the lab! 

**NOTE: Remember the location of the .pem file!**
## Running Ansible against an EC2
Let's the clone lab repo and change directories into it: `git clone https://github.com/digitalsoba/pathfinder-ec2-ansible.git && cd pathfinder-ec2-ansible`

Inside you should see the following files: `hosts, playbook.yaml, README.md, and files/index.html`

Before we begin this section of the lab, we need to configure a few things so we can properly connect to our EC2 instance.

Remember that .pem file we downloaded? We need to change the permissions before we connect to our EC2. Change directory into where the `.pem` is stored. Run `chmod 400 filename.pem` (Change filename.pem to the actual .pem file name). Also, take note of where this is located, we need to point ansible to this `.pem` file

Next, we need to open the `hosts` file in the repo. This file is an inventory that holds variables for Ansible to use. Under the server block, you should see a generic `192.168.1.1` IP address. Replace this with your EC2's Public IP or Public DNS. Next change `ansible_ssh_private_key_file=` to point to your `.pem` file. 

It should look something like this:
```
[server]
ec2-54-215-220-99.us-west-1.compute.amazonaws.com

[server:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_ssh_user=ubuntu
ansible_ssh_private_key_file=~/Downloads/pathfinder.pem
```

The goals of this lab are to deploy a webpage to our EC2 instance, the tasks are as follow:

* Update the cache repository (sudo apt-get update)
* Install the apache2 package
* Copy the index.html file into /var/www/html
* Set the group and owner of that file to www-data

When you're finished with the playbook, we can run it against our EC2. In the lab repo, run `ansible-playbook -i hosts playbook.yaml`. You might get a prompt about an ECDSA key fingerprint. Type yes and enter. When the playbook is finished, visit your EC2's IP or DNS address. 

## Creating a custom AMI
Great now we have a provisioned instance! From here, we can create an AMI that we can reuse in deployment. 

In the AWS Console, navigate back to the EC2 Dashboard and view our instances. Click the instance and verify there's a blue square next to its name (This means we've selected this instance). 

Click **Action** --> **Image** --> **Create Image**. A pop-up should now display. Give the image a name and description. Don't worry about the instance Volume settings, we can keep the default. Now we can click **Create Image**. Another window should show with the option to **View Pending Image**. This will take a few minutes, once the status is **available** we can launch a new AMI. 

Let's return to the EC2 Dashboard and launch another instance. When selecting an AMI, chose **My AMIs**, you should see your custom made AMI. Hit Select and use the following default settings: `t.2 micro, default instance details, default storage`
Under the tags, give it a new name. In security groups, we can use an existing security group. Select the one we've created for the previous instance. Review and launch the instance! Use the same keypair we've created. Now we can view your instances, wait for the EC2 to initialize. 

When the EC2 instance is running, visit its Public IP or DNS. 

## Clean up 
Congratulations! You've completed the Pathfinder program. It's time to clean up the lab. Select the two EC2 instances you've created, make sure they're both highlighted. Under **Actions** select **Instance State** and **Terminate Instances**. Confirm **Yes, Terminate** This will shut down and terminate your EC2 instances. Once terminated, we can select **Key Pairs** on the left navigation panel. Select the key pair name you created for the lab and delete it. 