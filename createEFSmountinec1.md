To create and mount an EFS (Elastic File System) volume on Oracle Linux 8 using AWS CLI commands, follow these steps:

1. Create an EFS file system:

```bash
aws efs create-file-system --performance-mode generalPurpose --throughput-mode bursting --encrypted
```

2. Create a mount target in your desired VPC and subnet:

```bash
aws efs create-mount-target --file-system-id fs-xxxxxxxx --subnet-id subnet-xxxxxxxx --security-groups sg-xxxxxxxx
```

3. Install the NFS client on your Oracle Linux 8 instance:

```bash
sudo dnf install -y nfs-utils
```

4. Create a mount point directory:

```bash
sudo mkdir /mnt/efs
```

5. Mount the EFS file system:

```bash
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-xxxxxxxx.efs.us-west-2.amazonaws.com:/ /mnt/efs
```

Replace `fs-xxxxxxxx` with your actual file system ID and adjust the region in the mount command if necessary.

6. To make the mount persistent across reboots, add an entry to /etc/fstab:

```bash
echo "fs-xxxxxxxx.efs.us-west-2.amazonaws.com:/ /mnt/efs nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 0 0" | sudo tee -a /etc/fstab
```

7. Verify the mount:

```bash
df -h /mnt/efs
```

Remember to replace placeholders like `fs-xxxxxxxx`, `subnet-xxxxxxxx`, and `sg-xxxxxxxx` with your actual EFS file system ID, subnet ID, and security group ID respectively[1][2].

Citations:
[1] https://www.youtube.com/watch?v=Aux37Nwe5nc
[2] https://docs.oracle.com/en/learn/ol-lvm/
[3] https://docs.oracle.com/en/operating-systems/oracle-linux/7/fsadmin/fsadmin-CreatingandManagingFileSystems.html
[4] https://stackoverflow.com/questions/46124006/how-to-configure-a-persistent-volume-claim-using-aws-efs-and-readwritemany
[5] https://stackoverflow.com/questions/61091913/how-do-i-create-a-folder-on-efs
[6] https://docs.aws.amazon.com/efs/latest/ug/mounting-fs-mount-helper-ec2-linux.html
[7] https://github.com/aws/efs-utils?tab=readme-ov-file
[8] https://repost.aws/knowledge-center/efs-mount-automount-unmount-steps
