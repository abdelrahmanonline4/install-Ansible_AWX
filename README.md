# install-Ansible_AWX
This repository provides scripts and configurations to install and set up Ansible AWX, an open-source web-based user interface, REST API, and task engine for Ansible

![1_ZRDVsat3i2-QFh3zE_ZxXg (1)](https://github.com/user-attachments/assets/52c7a5af-438c-4f41-9f05-d45f1e1fcaaa)

# Prerequisites
Before we dive into the installation process,
ensure that you have an Ubuntu server up and running.
Additionally, you’ll need root or sudo privileges to execute the necessary commands. 
# This setup has been done on an Ubuntu  virtual machine with 4 CPUs and 8 GB RAM.

# Step 1: Update and Upgrade System Packages

sudo apt update -y

sudo apt upgrade -y

# Step 2: Install Ansible and Required Packages

sudo apt install python-setuptools -y

sudo apt install python3-pip -y

sudo pip3 install ansible

ansible --version

pip3 install docker==6.1.3

sudo pip3 install docker-compose

docker-compose version

# Step 3: Grant Docker Access to the Current User

sudo usermod -aG docker $USER


# Step 4: Install Required Packages for AWX Setup

sudo apt install git vim pwgen -y


# Step 5: Clone the Ansible AWX Repository

sudo git clone https://github.com/ansible/awx.git --branch 17.0.1 --depth 1


# Step 6: Generate a Secret Key and Modify the Inventory File
cd awx/installer

pwgen -N 1 -s 30   #to get secretkey

sudo vi inventory
________________________________________________________________________________
localhost ansible_connection=local ansible_python_interpreter="/usr/bin/python3"

[all:vars]

#Remove these lines if you want to run a local image build

dockerhub_base=ansible

#Common Docker parameters

awx_task_hostname=awx

awx_web_hostname=awxweb

postgres_data_dir="~/.awx/pgdocker"

host_port=80

host_port_ssl=443

docker_compose_dir="~/.awx/awxcompose"

#PostgreSQL configuration

pg_username=awx

pg_password=awxpass

pg_database=awx

pg_port=5432

#Admin user and password

admin_user=admin

admin_password=adminpass


#AWX Secret key

secret_key=yourgeneratedsecretkey

#Create preload data for demonstration purposes

create_preload_data=True

#Additional optional settings can be configured below as neede
__________________________________________________________________
In the inventory file, replace the admin_password and secret_key with your desired values. Save the file by pressing Esc, then :wq.


# Step 7: Run the Ansible Playbook to Install AWX

ansible-playbook -i inventory install.yml

# Step 8: Access the AWX Web Interface

After the installation is complete, you can access the AWX web interface by navigating to http://your-server-ip in your web browser.
Use the user name admin and the admin password you specified in the inventory file during installation. Once you login you should be greeted with the following dashboard.

  ![image](https://github.com/user-attachments/assets/f672ddca-a874-4fd9-93cb-3e6e06a88adf)













