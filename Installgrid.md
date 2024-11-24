To install Oracle Grid Infrastructure from software stored in an AWS S3 bucket, follow these steps:

---

### **1. Prerequisites**
Ensure the following are in place:
- Oracle Grid user (`grid`) and required groups (`oinstall`, `asmadmin`, etc.) are created.
- Directories for Oracle Grid are set up (e.g., `/u01/app/grid`).
- AWS CLI is installed and configured on the system.
- Necessary kernel parameters and prerequisites for Oracle Grid Infrastructure are configured.

---

### **2. Download the Oracle Grid Software from S3**

#### **Step 1: Check lists AWS S3  Files**
Ensure you have attached S3 readonly access rold and configured AWS CLI:
```bash
aws s3 ls
```

#### **Step 2: Download the Grid Software**
Create a directory to store the downloaded software:
```bash
sudo mkdir -p /u01/software
sudo chown grid:oinstall /u01/software
```

Switch to the `grid` user:
```bash
sudo su - grid
```

Download the Grid software:
```bash
aws s3 cp s3://oraclesoftware123/LINUX.X64_193000_grid_home.zip /u01/software/
```

---

### **3. Extract the Grid Software**
Navigate to the software directory:
```bash
cd /u01/software
```

Extract the downloaded ZIP file:
```bash
unzip LINUX.X64_193000_grid_home.zip -d /u01/app/grid
```

---

### **4. Set Permissions**
Ensure that the extracted files have the correct ownership and permissions:
```bash
sudo chown -R grid:oinstall /u01/app/grid
sudo chmod -R 775 /u01/app/grid
```

---

### **5. Run the Oracle Grid Installer**
Switch to the `grid` user:
```bash
sudo su - grid
```

Navigate to the `gridSetup.sh` script directory:
```bash
cd /u01/app/grid/gridSetup
```

Run the installer:
```bash
./gridSetup.sh
```

---

### **6. Follow the Installer Prompts**
The Oracle Grid installer will launch. Follow the steps:
1. **Select Configuration**:
   - Choose whether to install Grid Infrastructure for a cluster or standalone server.
2. **Select Installation Path**:
   - Confirm the `ORACLE_BASE` and `GRID_HOME`.
3. **Set up ASM (Automatic Storage Management)**:
   - Configure ASM disk groups.
4. **Review Prerequisites**:
   - Address any prerequisite warnings or errors.
5. **Complete Installation**:
   - Review the summary and start the installation.

---

### **7. Verify the Installation**
After installation, verify that Oracle Grid Infrastructure is running:
```bash
crsctl check cluster
```

---

### **8. Clean Up (Optional)**
After the installation, you can delete the downloaded ZIP file to save space:
```bash
rm -f /u01/software/LINUX.X64_193000_grid_home.zip
```

---
