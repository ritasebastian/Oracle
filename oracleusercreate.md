To create an Oracle user on Oracle Linux 8 for Oracle Database installation, follow these steps:

---

### **1. Create Groups for Oracle**
Oracle requires specific operating system groups for managing permissions.

#### Create the Primary Group (`oinstall`):
```bash
sudo groupadd oinstall
```

#### Create the DBA Group:
```bash
sudo groupadd dba
```

#### (Optional) Create the Oper Group:
```bash
sudo groupadd oper
```

---

### **2. Create the Oracle User**
Create the Oracle user and assign it to the groups created above:
```bash
sudo useradd -g oinstall -G dba,oper oracle
```

Set a password for the Oracle user:
```bash
sudo passwd oracle
```

---

### **3. Create Directories for Oracle**
Create the required directories for Oracle software installation:
```bash
sudo mkdir -p /u01/app/oracle
sudo mkdir -p /u01/app/oraInventory
sudo mkdir -p /u01/app/oracle/product/19.0.0/dbhome_1
```

Set the ownership and permissions for the directories:
```bash
sudo chown -R oracle:oinstall /u01
sudo chmod -R 775 /u01
```

---

### **4. Configure Oracle User Environment**
Switch to the `oracle` user:
```bash
sudo su - oracle
```

Edit the `.bash_profile` for the `oracle` user:
```bash
vi ~/.bash_profile
```

Add the following lines to configure the Oracle environment variables:
```bash
# Oracle Environment Variables
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=/u01/app/oracle/product/19.0.0/dbhome_1
export ORACLE_SID=orcl
export PATH=$PATH:$ORACLE_HOME/bin
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export CLASSPATH=$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
export CV_ASSUME_DISTID=OEL8.0
```

Save and exit the file. Apply the changes:
```bash
source ~/.bash_profile
```

---

### **5. Verify the Setup**
- Check the Oracle user's groups:
  ```bash
  id oracle
  ```
- Verify environment variables:
  ```bash
  echo $ORACLE_HOME
  echo $ORACLE_SID
  ```

---

### **6. (Optional) Adjust Kernel Parameters for Oracle**
Edit `/etc/sysctl.conf` and set the required kernel parameters:
```bash
sudo vi /etc/sysctl.conf
```

Add the following parameters:
```plaintext
fs.file-max = 6815744
kernel.sem = 250 32000 100 128
kernel.shmall = 2097152
kernel.shmmax = 1073741824
kernel.shmmni = 4096
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
```

Apply the changes:
```bash
sudo sysctl -p
```

---

