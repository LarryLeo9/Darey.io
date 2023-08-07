# DEVOPS TOOLING WEBSITE SOLUTION

In previous [Project 6(https://www.dareyio.com/docs/project-6-step-1/) you implemented a WordPress based solution that is ready to be filled with content and can be used as a full fledged website or blog. Moving further we will add some more value to our solutions that your DevOps team could utilize. We want to introduce a set of DevOps tools that will help our team in day to day activities in managing, developing, testing, deploying and monitoring different projects.

The tools we want our team to be able to use are well known and widely used by multiple DevOps teams, so we will introduce a single DevOps Tooling Solution that will consist of:

1. Jenkins – free and open source automation server used to build CI/CD pipelines.

2. Kubernetes – an open-source container-orchestration system for automating computer application deployment, scaling, and management.

3. Jfrog Artifactory – Universal Repository Manager supporting all major packaging formats, build tools and CI servers. Artifactory.

4. Rancher – an open source software platform that enables organizations to run and manage Docker and Kubernetes in production.

5. Grafana – a multi-platform open source analytics and interactive visualization web application.

6. Prometheus – An open-source monitoring system with a dimensional data model, flexible query language, efficient time series database and modern alerting approach.

7. Kibana – Kibana is a free and open user interface that lets you visualize your Elasticsearch data and navigate the Elastic Stack.

Note: Do not feel overwhelmed by all the tools and technologies listed above, we will gradually get ourselves familiar with them in upcoming projects!

### Side Self Study

Read about Network-attached storage (NAS), Storage Area Network (SAN) and related protocols like NFS, (s)FTP, SMB, iSCSI. Explore what Block-level storage is and how it is used by Cloud Service providers, know the difference from Object storage.

On the example of AWS services understand the difference between Block Storage, Object Storage and Network File System.

### Setup and technologies used in Project 7

As a member of a DevOps team, you will implement a tooling website solution which makes access to DevOps tools within the corporate infrastructure easily accessible.

In this project you will implement a solution that consists of following components:

1. Infrastructure: AWS

2. Webserver Linux: Red Hat Enterprise Linux 8

3. Database Server: Ubuntu 20.04 + MySQL

4. Storage Server: Red Hat Enterprise Linux 8 + NFS Server

5. Programming Language: PHP

6. Code Repository: GitHub

For Rhel 8 server use this ami RHEL-8.6.0_HVM-20220503-x86_64-2-Hourly2-GP2 (ami-035c5dc086849b5de)

<img width="745" alt="Screenshot 2023-08-04 at 16 10 37" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/a05ef945-0f18-4e0f-b1ed-e76c1d3a725f">

<img width="745" alt="Screenshot 2023-08-04 at 16 11 58" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/74f5a31a-c9f6-4081-91c6-97bb56828060">

<img width="746" alt="Screenshot 2023-08-04 at 16 12 43" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/1aada68b-4fc0-4a17-b525-13788e26e777">

<img width="746" alt="Screenshot 2023-08-04 at 16 13 37" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/922d5cf1-9fb7-47d4-93a6-84305dc26046">

On the diagram below you can see a common pattern where several stateless Web Servers share a common database and also access the same files using Network File Sytem (NFS) as a shared file storage. Even though the NFS server might be located on a completely separate hardware – for Web Servers it look like a local file system from where they can serve the same files.

<img width="717" alt="Screenshot 2023-08-04 at 16 15 33" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/b4098bde-b6c6-43d7-86a7-4ed61de72921">

It is important to know what storage solution is suitable for what use cases, for this – you need to answer following questions: what data will be stored, in what format, how this data will be accessed, by whom, from where, how frequently, etc. Base on this you will be able to choose the right storage system for your solution.

### STEP 1 – PREPARE NFS SERVER

  1. Spin up a new EC2 instance with RHEL Linux 8 Operating System.

  2. Based on your LVM experience from Project 6, Configure LVM on the Server.

  <img width="413" alt="Screenshot 2023-08-04 at 19 16 56" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/02fb796b-1a33-422d-86d7-6c21123e422f">

  <img width="625" alt="Screenshot 2023-08-04 at 19 18 32" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/a0038fa3-0e92-4a2c-bc46-27a19f4748b3">
  
  <img width="466" alt="Screenshot 2023-08-04 at 19 20 30" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/5b71e13f-6392-4089-96c4-f6f282f6ea8c">

  <img width="783" alt="Screenshot 2023-08-04 at 19 22 24" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/9872fefb-fa1d-4386-9c35-d188af01560f">

  <img width="392" alt="Screenshot 2023-08-04 at 19 23 35" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/fc4af1d4-e6ad-46df-83dd-0b22fe46e214">

  <img width="385" alt="Screenshot 2023-08-04 at 19 24 43" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/0c9343d2-c7ba-4c7b-a050-e4eaf913be91">

  <img width="618" alt="Screenshot 2023-08-04 at 19 25 36" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/dae1d1c3-0f46-4545-a326-b04b42450624">

  <img width="354" alt="Screenshot 2023-08-04 at 19 26 24" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/2ff8c38d-6f87-426b-8dba-e60cf6a6d62a">

  <img width="708" alt="Screenshot 2023-08-04 at 19 27 13" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/999d9825-0788-4224-a25e-67462226f20d">

  <img width="728" alt="Screenshot 2023-08-04 at 19 28 25" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/54a3e857-92b7-41a3-b641-4b53e961ac80">


. Instead of formating the disks as ext4 you will have to format them as xfs

  <img width="621" alt="Screenshot 2023-08-04 at 19 30 15" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/adf200ae-5ab4-49fe-b3a8-8d2fe3b8e86a">

  <img width="621" alt="Screenshot 2023-08-04 at 19 31 18" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/b697073f-65a9-401e-b200-2968542ff2da">

  <img width="621" alt="Screenshot 2023-08-04 at 19 32 13" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/d9d000c1-1a69-4e48-8789-103bc6a8fcaf">


. Ensure there are 3 Logical Volumes. lv-opt lv-apps, and lv-logs

  <img width="1059" alt="Screenshot 2023-08-04 at 19 36 13" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/28a593ba-130a-4c45-93bd-78140fbc83aa">


. Create mount points on /mnt directory for the logical volumes as follow:
    
    Mount lv-apps on /mnt/apps – To be used by webservers
    
    Mount lv-logs on /mnt/logs – To be used by webserver logs
    
    Mount lv-opt on /mnt/opt – To be used by Jenkins server in Project 8

  <img width="422" alt="Screenshot 2023-08-04 at 19 37 43" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/f0a37a60-67f9-48c5-bfae-2142ada33756">

  <img width="618" alt="Screenshot 2023-08-04 at 19 39 08" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/ee884d8d-7b8a-4777-b433-585a42631a64">


3. Install NFS server, configure it to start on reboot and make sure it is u and running

       sudo yum -y update

       sudo yum install nfs-utils -y

       sudo systemctl start nfs-server.service

       sudo systemctl enable nfs-server.service

       sudo systemctl status nfs-server.service

4. Export the mounts for webservers’ subnet cidr to connect as clients. For simplicity, you will install your all three Web Servers inside the same subnet, but in production set up you would probably want to separate each tier inside its own subnet for higher level of security.
To check your subnet cidr – open your EC2 details in AWS web console and locate ‘Networking’ tab and open a Subnet link:

<img width="746" alt="Screenshot 2023-08-04 at 16 28 25" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/965fc5f1-81a7-41ca-a14b-246ab1261e9b">

Make sure we set up permission that will allow our Web servers to read, write and execute files on NFS:

    sudo chown -R nobody: /mnt/apps
    sudo chown -R nobody: /mnt/logs
    sudo chown -R nobody: /mnt/opt

    sudo chmod -R 777 /mnt/apps
    sudo chmod -R 777 /mnt/logs
    sudo chmod -R 777 /mnt/opt

    sudo systemctl restart nfs-server.service

  <img width="587" alt="Screenshot 2023-08-04 at 19 46 22" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/0f91a121-c502-46f4-b4ff-ce0d3dbfe3b1">


6. Configure access to NFS for clients within the same subnet (example of Subnet CIDR – 172.31.32.0/20 ):

    sudo vi /etc/exports

    /mnt/apps < Subnet-CIDR > (rw,sync,no_all_squash,no_root_squash)
    
    /mnt/logs < Subnet-CIDR > (rw,sync,no_all_squash,no_root_squash)
    
    /mnt/opt < Subnet-CIDR > (rw,sync,no_all_squash,no_root_squash)

  <img width="542" alt="Screenshot 2023-08-04 at 19 56 50" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/c9e5a15b-1499-44dc-9930-39196353f22b">

    Esc + :wq!

    sudo exportfs -arv

  <img width="424" alt="Screenshot 2023-08-04 at 19 47 46" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/fdec60ed-4cca-4c75-b6bd-f109f9dfc9a2">


7. Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)

        rpcinfo -p | grep nfs

  <img width="410" alt="Screenshot 2023-08-04 at 19 48 39" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/ca69f555-bd23-4365-941f-0643b560deb8">

  Important note: In order for NFS server to be accessible from your client, you must also open following ports: TCP 111, UDP 111, UDP 2049

  <img width="739" alt="Screenshot 2023-08-04 at 19 50 42" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/aa25afe3-3ef3-474c-919a-dbc95be5a4db">



### STEP 2 — CONFIGURE THE DATABASE SERVER

By now you should know how to install and configure a MySQL DBMS to work with remote Web Server

Install MySQL server

  <img width="664" alt="Screenshot 2023-08-04 at 20 01 51" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/294cfe51-67b5-4c3b-bada-e7bf4d9b4eb6">

  <img width="664" alt="Screenshot 2023-08-04 at 20 03 25" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/f098142f-09fa-471e-99e0-3579aa3d82c8">

Create a database and name it tooling

  <img width="555" alt="Screenshot 2023-08-04 at 20 06 16" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/131fe9a4-5508-4fee-aed6-fb003364acfa">

Create a database user and name it webaccess

  <img width="506" alt="Screenshot 2023-08-04 at 20 07 47" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/7e5aef9b-09e5-4b91-b968-ee122c53c1e8">

Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr

  <img width="519" alt="Screenshot 2023-08-04 at 20 09 14" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/670c0967-0f18-4fc0-823a-426a0213d85d">

  <img width="264" alt="Screenshot 2023-08-04 at 20 10 12" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/5d3baf51-c3f9-4088-b2a2-1541f176944b">

  <img width="299" alt="Screenshot 2023-08-04 at 20 11 02" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/c8192676-993d-4da6-be1c-3c065d58d8b0">


### Step 3 — Prepare the Web Servers

We need to make sure that our Web Servers can serve the same content from shared storage solutions, in our case – NFS Server and MySQL database.

You already know that one DB can be accessed for reads and writes by multiple clients. For storing shared files that our Web Servers will use – we will utilize NFS and mount previously created Logical Volume lv-apps to the folder where Apache stores files to be served to the users (/var/www).

This approach will make our Web Servers stateless, which means we will be able to add new ones or remove them whenever we need, and the integrity of the data (in the database and on NFS) will be preserved.

During the next steps we will do following:

  . Configure NFS client (this step must be done on all three servers)

  . Deploy a Tooling application to our Web Servers into a shared NFS folder

  . Configure the Web Servers to work with a single MySQL database

1. Launch a new EC2 instance with RHEL 8 Operating System

2. Install NFS client

          sudo yum install nfs-utils nfs4-acl-tools -y

3. Mount /var/www/ and target the NFS server’s export for apps
   
        sudo mkdir /var/www

        sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www

   <img width="647" alt="Screenshot 2023-08-04 at 21 12 06" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/729d29fe-ba14-4ee6-b3f7-dc285168608b">


5. Verify that NFS was mounted successfully by running df -h. Make sure that the changes will persist on Web Server after reboot:

        sudo vi /etc/fstab

  add following line

    <NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0

5. Install Remi’s repository, Apache and PHP

        sudo yum install httpd -y

        sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

        sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

        sudo dnf module reset php

        sudo dnf module enable php:remi-7.4

        sudo dnf install php php-opcache php-gd php-curl php-mysqlnd

        sudo systemctl start php-fpm

        sudo systemctl enable php-fpm

        setsebool -P httpd_execmem 1

  Repeat steps 1-5 for another 2 Web Servers.

6. Verify that Apache files and directories are available on the Web Server in /var/www and also on the NFS server in /mnt/apps. If you see the same files – it means NFS is mounted correctly. You can try to create a new file touch test.txt from one server and check if the same file is accessible from other Web Servers.

7. Locate the log folder for Apache on the Web Server and mount it to NFS server’s export for logs. Repeat step №4 to make sure the mount point will persist after reboot.

        sudo mount -t nfs -o rw,nosuid < NFS-Server-Private-IP-Address >:/mnt/logs /var/log/httpdd
   
8. Fork the tooling source code from Darey.io Github Account to your Github account. (Learn how to fork a repo here)

     git clone https://github.com/darey-io/tooling.git

9. Deploy the tooling website’s code to the Webserver. Ensure that the html folder from the repository is deployed to /var/www/html

   <img width="725" alt="Screenshot 2023-08-04 at 22 14 57" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/0f6e7916-d923-4889-a0f9-b27bf88c2fd2">

Note 1: Do not forget to open TCP port 80 on the Web Server.

Note 2: If you encounter 403 Error – check permissions to your /var/www/html folder and also disable SELinux sudo setenforce 0

To make this change permanent – open following config file sudo vi /etc/sysconfig/selinux and set SELINUX=disabledthen restrt httpd.

<img width="728" alt="Screenshot 2023-08-04 at 20 24 29" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/72a5a1ba-4c07-4146-9edd-816a8218a1dc">

10. Update the website’s configuration to connect to the database (in /var/www/html/functions.php file). Apply tooling-db.sql script to your database using this command mysql -h <databse-private-ip> -u <db-username> -p <db-pasword> < tooling-db.sql

11. Create in MySQL a new admin user with username: myuser and password: password:

    INSERT INTO ‘users’ (‘id’, ‘username’, ‘password’, ’email’, ‘user_type’, ‘status’) VALUES
-> (1, ‘myuser’, ‘5f4dcc3b5aa765d61d8327deb882cf99’, ‘user@mail.com’, ‘admin’, ‘1’);

12. Open the website in your browser http://<Web-Server-Public-IP-Address-or-Public-DNS-Name>/index.php and make sure you can login into the website with myuser user.



