To create an Oracle Grid Infrastructure (GI) user on a Linux system, follow these steps:

---

### **1. Check Prerequisites**
Before creating the Oracle Grid user, ensure the following:
- You have root or sudo privileges.
- Your system meets the Oracle Grid Infrastructure prerequisites.

---

### **2. Create the Oracle Grid User**

#### **Step 1: Create the User Group**
Create the necessary operating system groups for Oracle Grid Infrastructure:
```bash
sudo groupadd oinstall
sudo groupadd asmadmin
sudo groupadd asmdba
sudo groupadd asmoper
```

#### **Step 2: Create the Grid User**
Create the Oracle Grid user (`grid`) and assign it to the groups:
```bash
sudo useradd -g oinstall -G asmadmin,asmdba,asmoper grid
```

#### **Step 3: Set a Password for the Grid User**
Set a password for the `grid` user:
```bash
sudo passwd grid
```

---

### **3. Configure User Environment**

#### **Step 1: Set up the `.bash_profile` for the Grid User**
Switch to the `grid` user and edit its `.bash_profile`:
```bash
sudo su - grid
ssh-keygen -t rsa -b 2048
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
vi ~/.bash_profile
```

Add the following lines to configure the environment variables:
```bash
# Oracle Grid Infrastructure environment variables
export ORACLE_BASE=/u01/app/grid
export ORACLE_HOME=/u01/app/19.0.0/grid
export ORACLE_SID=+ASM
export PATH=$PATH:$ORACLE_HOME/bin
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
export CLASSPATH=$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
export CV_ASSUME_DISTID=OEL8.0
```

Save and exit the editor. Apply the changes:
```bash
source ~/.bash_profile
```

#### **Step 2: Create Oracle Base Directories**
Create the necessary directories for Oracle Grid and set permissions:
```bash
sudo mkdir -p /u01/app/grid
sudo chown -R grid:oinstall /u01/app/grid
sudo chmod -R 775 /u01/app/grid
```

---

### **4. Verify the User**
Log in as the `grid` user to ensure everything is set up properly:
```bash
su - grid
echo $ORACLE_HOME
```

The output should display the `ORACLE_HOME` path.

---

