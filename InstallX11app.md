To enable X11 applications on an Oracle Linux 8.9 (OL8.9) EC2 instance, follow these steps:

1. Install necessary X11 packages:
   ```bash
   sudo dnf config-manager --enable ol8_codeready_builder
   sudo dnf install -y xorg-x11-apps xorg-x11-xauth
   ```

2. Configure SSH server for X11 forwarding:
   Edit /etc/ssh/sshd_config and ensure these lines are present and uncommented:
   ```
   X11Forwarding yes
   X11UseLocalhost no
   ```

3. Restart the SSH service:
   ```bash
   sudo systemctl restart sshd
   ```

4. On your local machine:
   - Install an X11 server (XQuartz for Mac or Xming for Windows)
   - Enable X11 forwarding in your SSH client

5. Connect to the EC2 instance using SSH with X11 forwarding:
   ```bash
   sudo mkdir -p /home/oracle/.ssh
   sudo mkdir -p /home/grid/.ssh
   sudo cp /home/ec2-user/.ssh/authorized_keys /home/oracle/.ssh
   sudo cp /home/ec2-user/.ssh/authorized_keys /home/grid/.ssh
   sudo chown -R oracle:oinstall /home/oracle/.ssh
   sudo chown -R grid:oinstall /home/grid/.ssh
   ssh -i "2024-oregan.pem" -X ec2-user@ec2-54-187-120-22.us-west-2.compute.amazonaws.com
   ```

6. Test X11 forwarding by running an X11 application, such as:
   ```bash
   xclock
   ```

The error message indicates that the .Xauthority file is missing on the EC2 instance. To resolve this issue and enable X11 forwarding, follow these steps:

1. Connect to your EC2 instance:
   ```bash
   ssh -i "2024-oregan.pem" -X ec2-user@ec2-54-187-120-22.us-west-2.compute.amazonaws.com
   ```

2. Once connected, create the .Xauthority file:
   ```bash
   touch ~/.Xauthority
   ```

3. Set the correct permissions:
   ```bash
   chmod 600 ~/.Xauthority
   ```

4. Generate a new authentication cookie:
   ```bash
   xauth generate :0 . trusted
   ```

5. Ensure the DISPLAY environment variable is set correctly:
   ```bash
   export DISPLAY=:0
   ```

6. Test X11 forwarding by running a simple X application:
   ```bash
   xclock
   ```

If the xclock application appears on your local machine, X11 forwarding is working correctly. If you encounter any issues, make sure:

- X11 forwarding is enabled in the SSH server config (/etc/ssh/sshd_config)
- Your local machine has an X11 server installed and running
- The EC2 security group allows inbound SSH traffic from your IP address

Remember to install any necessary X11 packages on the EC2 instance if they're not already present:

```bash
sudo yum install -y xorg-x11-xauth xorg-x11-apps
```
