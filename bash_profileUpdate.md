

### **Script: `update_bash_profile.sh`**
```bash
#!/bin/bash

# Backup existing .bash_profile
if [ -f ~/.bash_profile ]; then
  cp ~/.bash_profile ~/.bash_profile.bak
  echo "Backup of .bash_profile created at ~/.bash_profile.bak"
fi

# Append Oracle settings using EOF format
cat <<EOF >> ~/.bash_profile

# Oracle Settings
export TMP=/tmp
export TMPDIR=\$TMP

export ORACLE_HOSTNAME=ol8-19.localdomain
export ORACLE_UNQNAME=cdb1
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=\$ORACLE_BASE/product/19.0.0/dbhome_1
export ORA_INVENTORY=/u01/app/oraInventory
export ORACLE_SID=cdb1
export PDB_NAME=pdb1
export DATA_DIR=/u02/oradata

export PATH=/usr/sbin:/usr/local/bin:\$PATH
export PATH=\$ORACLE_HOME/bin:\$PATH

export LD_LIBRARY_PATH=\$ORACLE_HOME/lib:/lib:/usr/lib
export CLASSPATH=\$ORACLE_HOME/jlib:\$ORACLE_HOME/rdbms/jlib
EOF

echo "Oracle settings appended to ~/.bash_profile"

# Source the updated .bash_profile
source ~/.bash_profile
echo ".bash_profile updated and sourced."
```

---

### **Steps to Use the Script**
1. **Create the script**:
   ```bash
   nano update_bash_profile.sh
   ```
2. **Paste the script** and save the file.
3. **Make it executable**:
   ```bash
   chmod +x update_bash_profile.sh
   ```
4. **Run the script**:
   ```bash
   ./update_bash_profile.sh
   ```

---

### **What the Script Does**
1. Creates a backup of the existing `.bash_profile` as `.bash_profile.bak`.
2. Appends the Oracle settings using a `cat <<EOF` block to ensure correct formatting.
3. Sources the updated `.bash_profile` to apply the changes immediately.

This ensures the content is appended in a clean, readable format. Let me know if you need further adjustments!
