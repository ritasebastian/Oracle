Steps to create an AWS EC2 instance with Oracle Linux, attach a security group, and assign an IAM role:

## 1. Create AWS EC2 Instance

1. Sign in to the AWS Management Console and navigate to the EC2 dashboard.

2. Click "Launch Instance" to start the instance creation process.

3. In the "Choose an Amazon Machine Image (AMI)" section, select "AWS Marketplace" or "Community AMIs"[1].

4. In the search bar, enter the owner ID "131827586825" to find Oracle-provided Oracle Linux AMIs[1].

5. Choose either Oracle Linux 8 or 9 from the search results.

6. Select the t2.large instance type[1].

7. Configure instance details, such as the number of instances and network settings.

8. Add storage as needed for your instance.

9. Add tags to help organize and manage your EC2 resources.

10. Proceed to configure the security group in the next step.

## 2. Attach Security Group

1. In the "Configure Security Group" section, create a new security group.

2. Add the following inbound rules:
   - SSH (port 22) from your IP address
   - Oracle database port (1521) from necessary sources
   - EFS port (2049) for NFS access

3. Review and launch the instance.

4. Select an existing key pair or create a new one to securely connect to your instance.

5. Launch the instance.

## 3. Create and Attach IAM Role

1. Open the IAM console in a new tab.

2. Click on "Roles" in the left navigation pane and then "Create role".

3. Select "AWS service" as the trusted entity and choose "EC2" as the use case.

4. In the permissions section, search for and attach the "AmazonS3ReadOnlyAccess" policy.

5. Give the role a name (e.g., "EC2S3ReadOnlyRole") and create it.

6. Return to the EC2 console and select your newly created instance.

7. Click "Actions" > "Security" > "Modify IAM role".

8. Select the role you just created from the dropdown menu.

9. Click "Save" to attach the role to your EC2 instance.

Your Oracle Linux EC2 instance is now set up with the appropriate security group and IAM role for S3 read-only access.

Citations:
[1] https://www.youtube.com/watch?v=6kM_2P53qr8
[2] https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.Oracle.html
[3] https://forums-stg.oracle.com/ords/r/apexds/community/q?question=launch-an-oracle-linux-instance-in-aws-9462
[4] https://blogs.oracle.com/linux/post/running-oracle-linux-in-public-clouds
[5] https://aws.amazon.com/marketplace/pp/prodview-uddeiwhf2l26w
[6] https://forums.oracle.com/ords/r/apexds/community/q?question=launch-an-oracle-linux-instance-in-aws-9462
[7] https://aws.amazon.com/marketplace/pp/prodview-n5x4eougzrol6
[8] https://aws.amazon.com/ec2/instance-types/?ef_id=CjwKCAiAkfucBhBBEiwAFjbkrxZENs26cYbLv8odnxU83aouewDwhBwbsI_WPG6QtC_M2QXntFzqqRoClUEQAvD_BwE%3AG%3As&s_kwcid=AL%214422%213%21639434028189%21e%21%21g%21%21aws+ec2+server&sc_channel=ps&trk=fa4fa111-b43a-4e17-8976-4c87239c8376
