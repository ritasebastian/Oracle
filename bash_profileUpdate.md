

### ** `update_bash_profile.sh`**
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

