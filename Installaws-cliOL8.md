To install the AWS CLI on Oracle Linux 8, follow these steps:

---

### **1. Check and Update System Packages**
Ensure your system is up-to-date:
```bash
sudo yum update -y
```

---

### **2. Download the AWS CLI Installer**
Download the latest AWS CLI version 2 installer:
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

---

### **3. Install `unzip` (if not already installed)**
If `unzip` is not installed, install it first:
```bash
sudo yum install -y unzip
```

---

### **4. Unzip the Installer**
Extract the downloaded ZIP file:
```bash
unzip awscliv2.zip
```

---

### **5. Run the AWS CLI Installer**
Install the AWS CLI using the extracted files:
```bash
sudo ./aws/install
```

---

### **6. Verify the Installation**
Confirm that the AWS CLI is installed correctly:
```bash
aws --version
```
This should output something like:
```
aws-cli/2.x.x Python/3.x.x Linux/x86_64
```

---

### **7. Configure the AWS CLI (Optional)**
If this is your first time using the AWS CLI, configure it by providing your AWS credentials:
```bash
aws configure
```
You will be prompted to enter:
- Access Key ID
- Secret Access Key
- Default region
- Output format (e.g., `json`)

---

### **8. Clean Up Installation Files**
Remove the downloaded and extracted files:
```bash
rm -rf awscliv2.zip aws
```

---

The AWS CLI is now installed on Oracle Linux 8 and ready for use. Let me know if you encounter any issues!
