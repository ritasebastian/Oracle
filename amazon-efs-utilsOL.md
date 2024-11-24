The `amazon-efs-utils` package is not available in the default Oracle Linux 8 repositories. To install it, you can build the package from the source provided by AWS. Here's how to do it:

**1. Install Required Dependencies**

First, ensure that your system has the necessary tools and libraries:

```bash
sudo yum install -y git rpm-build make rust cargo openssl-devel
```

**2. Clone the `amazon-efs-utils` Repository**

Next, clone the `amazon-efs-utils` repository from GitHub:

```bash
git clone https://github.com/aws/efs-utils
```

**3. Build the RPM Package**

Navigate to the cloned directory and build the RPM package:

```bash
cd efs-utils
make rpm
```

**4. Install the Built RPM Package**

After the build process completes, install the package:

```bash
sudo yum install -y build/amazon-efs-utils*rpm
```

**5. Verify the Installation**

Confirm that the `amazon-efs-utils` package is installed:

```bash
rpm -qi amazon-efs-utils
```

This process compiles and installs the `amazon-efs-utils` package on your Oracle Linux 8 system, enabling you to mount Amazon EFS file systems.

```bash
sudo mkdir -p /orcl-arch
```
```bash
sudo mount -t efs -o tls fs-080066337fa685f02:/ /orcl-arch # replace the file system id
```

To add the mount configuration to `/etc/fstab` so the EFS file system mounts automatically at boot, you can use the following command:

```bash
echo "fs-080066337fa685f02.efs.us-west-2.amazonaws.com:/ /efs nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 0 0" | sudo tee -a /etc/fstab
```

### Explanation of the Command:
1. **EFS DNS Name**: `fs-080066337fa685f02.efs.us-west-2.amazonaws.com:/` - The EFS file system DNS.
2. **Mount Point**: `/efs` - The directory where the file system will be mounted.
3. **Filesystem Type**: `nfs4` - Mounts the file system as an NFSv4.1 type.
4. **Options**:
   - `nfsvers=4.1`: Specifies the NFS version.
   - `rsize=1048576,wsize=1048576`: Sets the read and write buffer sizes.
   - `hard`: Forces the system to retry if a server becomes unreachable.
   - `timeo=600`: Sets the timeout value to 600 deciseconds.
   - `retrans=2`: Limits the number of retransmissions to 2.
   - `noresvport`: Uses a non-reserved port for the NFS client.
5. **Dump and Pass Options**: `0 0` - Disables dump and fsck checks.

### Apply the Changes:
After adding the entry, apply it without rebooting:
```bash
sudo mount -a
```

### Verify the Mount:
Check if the EFS is mounted correctly:
```bash
df -h
```

This ensures the EFS file system is automatically mounted on boot with the specified options.
