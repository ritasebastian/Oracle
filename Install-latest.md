
# Set the hostname
NEW_HOSTNAME="orcl"
echo "Setting hostname to $NEW_HOSTNAME..."
hostnamectl set-hostname $NEW_HOSTNAME

# Update /etc/hosts
echo "Updating /etc/hosts..."
HOSTS_ENTRY="35.95.35.95 $NEW_HOSTNAME"
if ! grep -q "$NEW_HOSTNAME" /etc/hosts; then
    echo "$HOSTS_ENTRY" >> /etc/hosts
else
    echo "/etc/hosts already contains $NEW_HOSTNAME"
fi

sed -i s/SELINUX=enforcing/SELINUX=disabled/g /etc/selinux/config

sudo yum install -y binutils compat-libcap1 compat-libstdc++ glibc glibc-devel libaio libaio-devel make gcc gcc-c++
sudo yum install -y smartmontools sysstat unixODBC libX11 libXtst libXi libXxf86vm alsa-lib xorg-x11-utils

# Install oracle prereqisites  
 ```bash
sudo dnf install oraclelinux-release-el8
 sudo dnf install oracle-database-preinstall-19c -y
sudo dnf config-manager --set-enabled ol8_baseos_latest ol8_appstream ol8_addons
sudo dnf groupinstall "Development Tools" -y
sudo dnf install libaio-devel unixODBC-devel -y
wget https://cdn.amazonlinux.com/2/core/2.0/x86_64/6b0225ccc542f3834c95733dcf321ab9f1e77e6ca6817469771a8af7c49efe6c/../../../../../blobstore/e6eb2b14d86195fe7d1f603a87a78df99e2687b9b0ccb4ab10385f75bb57d5a6/compat-libcap1-1.10-7.amzn2.x86_64.rpm
rpm -ivh compat-libcap1-1.10-7.amzn2.x86_64.rpm
 ```
# Install oracleasm-support rpm package:
 ```bash
sudo dnf --enablerepo=ol8_addons -y install oracleasm-support
wget http://vault.centos.org/7.9.2009/os/x86_64/Packages/compat-libcap1-1.10-7.el7.x86_64.rpm
sudo dnf localinstall compat-libcap1-1.10-7.el7.x86_64.rpm -y
rpm -q --qf '%{NAME}-%{VERSION}-%{RELEASE} (%{ARCH})\n' binutils compat-libcap1 compat-libstdc++ glibc glibc-devel libaio libaio-devel make gcc gcc-c++ smartmontools sysstat unixODBC
/u01/app/grid/runcluvfy.sh stage -pre crsinst -n localhost -verbose

 ```
# Update the kernel
```bash
 sudo dnf update -y
 ```
# Install aws cli
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
# Create grid and oracle user with it profile(s)

# Connect to the EC2 instance using SSH with X11 forwarding
```bash
sudo mkdir -p /home/oracle/.ssh
sudo mkdir -p /home/grid/.ssh
sudo cp /home/ec2-user/.ssh/authorized_keys /home/oracle/.ssh
sudo cp /home/ec2-user/.ssh/authorized_keys /home/grid/.ssh
sudo chown -R oracle:oinstall /home/oracle/.ssh
sudo chown -R grid:oinstall /home/grid/.ssh
 ```

[grid@orcl ~]$ /u01/app/grid/runcluvfy.sh comp admprv -o user_equiv -n localhost -verbose

Verifying User Equivalence ...
  Node Name                             Status
  ------------------------------------  ------------------------
  localhost                             passed
Verifying User Equivalence ...PASSED

Verification of administrative privileges was successful.

CVU operation performed:      administrative privileges
Date:                         Nov 25, 2024 7:24:01 PM
CVU home:                     /u01/app/grid/
User:                         grid

