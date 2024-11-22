

#### **1. Create a Partition on `/dev/xvdb`**

1. Open `fdisk` for `/dev/xvdb`:
   ```bash
   sudo fdisk /dev/xvdb
   ```

2. Inside `fdisk`, follow these steps:
   - Type `n` and press **Enter** to create a new partition.
   - Choose **primary** (default option) and press **Enter**.
   - Press **Enter** to accept the default partition number (`1`).
   - Press **Enter** to accept the default start sector.
   - Press **Enter** to accept the default end sector (use the full disk).
   - Type `w` and press **Enter** to write the partition table and exit.

---

#### **2. Create a Partition on `/dev/xvdc`**

Repeat the same steps as above for `/dev/xvdc`:
```bash
sudo fdisk /dev/xvdc
```

---

#### **3. Reload the Partition Table**

Run the following command to reload the partition table:
```bash
sudo partprobe
```

---

#### **4. Verify the Partitions**

Run `lsblk` again to confirm the partitions have been created:
```bash
lsblk
```

You should see:
```plaintext
xvdb    202:16   0  50G  0 disk
└─xvdb1 202:17   0  50G  0 part
xvdc    202:32   0  50G  0 disk
└─xvdc1 202:33   0  50G  0 part
```

---

### **Steps to Configure Partitions for ASM**

#### **5. Mark the Partitions for ASM**
Use `oracleasm` to create ASM disks.

1. Initialize Oracle ASM (if not already done):
   ```bash
   sudo oracleasm configure -i
   sudo oracleasm init
   ```

2. Mark `/dev/xvdb1` as an ASM disk:
   ```bash
   sudo oracleasm createdisk DATA /dev/xvdb1
   ```

3. Mark `/dev/xvdc1` as an ASM disk:
   ```bash
   sudo oracleasm createdisk FRA /dev/xvdc1
   ```

---

#### **6. Verify ASM Disks**

Check the created ASM disks:
```bash
sudo oracleasm listdisks
```

You should see:
```plaintext
DATA
FRA
```

---

### **Common Issues and Fixes**

1. **If `oracleasm` cannot detect the disks**:
   - Ensure `oracleasm` is running:
     ```bash
     sudo systemctl status oracleasm
     ```
   - Restart the service:
     ```bash
     sudo systemctl restart oracleasm
     ```

2. **Ensure Correct Permissions**:
   - Verify permissions for the partitions:
     ```bash
     ls -l /dev/xvdb1 /dev/xvdc1
     ```
   - If necessary, adjust them:
     ```bash
     sudo chmod 660 /dev/xvdb1 /dev/xvdc1
     sudo chown oracle:oinstall /dev/xvdb1 /dev/xvdc1
     ```

3. **Reload Oracle ASM Driver**:
   If disks still don't show up in `oracleasm`, reload the driver:
   ```bash
   sudo oracleasm scandisks
   ```

