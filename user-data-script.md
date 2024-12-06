 ```bash
#!/bin/bash

# ************************************
# Initial Setup and Configuration
# ************************************

# Get the private IP address of the instance
PRIVATE_IP=$(curl -s http://169.254.169.254/latest/meta-data/local-ipv4)

# Define the hostname (you can customize this)
HOSTNAME="orcl"

# Set the hostname
hostnamectl set-hostname $HOSTNAME

# Update /etc/hosts with the private IP and hostname
echo "$PRIVATE_IP $HOSTNAME" > /etc/hosts

# ************************************
# Disable SELinux for Compatibility
# ************************************

# Disable SELinux temporarily (for the current session)
if command -v setenforce &> /dev/null; then
    setenforce 0
fi

# Disable SELinux permanently (modify /etc/selinux/config)
if [ -f /etc/selinux/config ]; then
    sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
fi

# ************************************
# Install Required Packages
# ************************************

# Update system packages
yum update -y

# Install essential system and Oracle dependencies
yum install -y binutils compat-libcap1 compat-libstdc++ glibc glibc-devel libaio libaio-devel make gcc gcc-c++ smartmontools sysstat unixODBC
yum install -y oracle-database-preinstall-19c.x86_64
yum install -y oracleasm oracleasm-support kmod-oracleasm
yum install -y compat-libstdc++-33.x86_64

# Install additional utilities
yum install -y unzip curl git

# ************************************
# Install AWS CLI
# ************************************

# Download the AWS CLI v2 installer
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

# Extract the AWS CLI zip file
unzip awscliv2.zip

# Run the installer
sudo ./aws/install

# ************************************
# Configure Users and Groups
# ************************************

# Create ASM groups
groupadd -g 54327 asmdba
groupadd -g 54328 asmoper
groupadd -g 54329 asmadmin

# Create the grid user and assign groups
useradd -u 54322 -g oinstall -G dba,asmdba,asmoper,asmadmin grid

# Modify the oracle user and assign groups
usermod -u 500 -g oinstall -G dba,oper,asmdba,asmoper,asmadmin,kmdba,dgdba,backupdba,racdba oracle

# ************************************
# Create Necessary Directories
# ************************************

# Grid infrastructure directories
mkdir -p /u01/app/grid/19c/grid_home
chown -R grid:oinstall /u01/app

# Oracle database directories
mkdir -p /u01/app/oracle/product/19c/db_1
chown -R oracle:oinstall /u01/app/oracle

# Software directory
mkdir -p /u01/software
chmod 777 /u01/software
# add oracle and grid into sudo 
echo "oracle ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
echo "oracle ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# ************************************
# Download Oracle Software from S3
# ************************************

# Copy Oracle software from S3 to local directories
sudo -u ec2-user aws s3 cp s3://oraclesoftware123/LINUX.X64_193000_grid_home.zip /u01/software/
sudo -u ec2-user aws s3 cp s3://oraclesoftware123/LINUX.X64_193000_db_home.zip /u01/software/

# ************************************
# Extract Oracle Software
# ************************************

# Unzip Grid Infrastructure software as the 'grid' user
sudo -u grid unzip /u01/software/LINUX.X64_193000_grid_home.zip -d /u01/app/grid/19c/grid_home

# Unzip Oracle Database software as the 'oracle' user
sudo -u oracle unzip /u01/software/LINUX.X64_193000_db_home.zip -d /u01/app/oracle/product/19c/db_1

# ************************************
# Load Sample Data into Oracle
# ************************************

# Clone sample data repository as the 'oracle' user
sudo -u oracle git clone https://github.com/ogobrecht/sample-data-sets-for-oracle.git

# ************************************
# Final Configuration
# ************************************

# Reload the network hostname service (optional but recommended)
systemctl restart systemd-hostnamed
```
