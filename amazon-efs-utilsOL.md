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


