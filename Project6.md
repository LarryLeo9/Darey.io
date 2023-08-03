# WEB SOLUTION WITH WORDPRESS

In this project you will be tasked to prepare storage infrastructure on two Linux servers and implement a basic web solution using WordPress. WordPress is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).

Project 6 consists of two parts:

1. Configure storage subsystem for Web and Database servers based on Linux OS. The focus of this part is to give you practical experience of working with disks, partitions and volumes in Linux.
2. Install WordPress and connect it to a remote MySQL database server. This part of the project will solidify your skills of deploying Web and DB tiers of Web solution.

As a DevOps engineer, your deep understanding of core components of web solutions and ability to troubleshoot them will play essential role in your further progress and development.

Three-tier Architecture
Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.

Three-tier Architecture is a client-server software architecture pattern that comprise of 3 separate layers.

<img width="686" alt="Screenshot 2023-08-03 at 17 30 21" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/24e640ad-34c8-41a3-a70d-6a2255389e42">


1. Presentation Layer (PL): This is the user interface such as the client server or browser on your laptop.
2. Business Layer (BL): This is the backend program that implements business logic. Application or Webserver
3. Data Access or Management Layer (DAL): This is the layer for computer data storage and data access. Database Server or File System Server such as FTP server, or NFS Server

In this project, you will have the hands-on experience that showcases Three-tier Architecture while also ensuring that the disks used to store files on the Linux servers are adequately partitioned and managed through programs such as gdisk and LVM respectively.

You will be working working with several storage and disk management concepts, to have a better understanding, watch following video:

Your 3-Tier Setup
1. A Laptop or PC to serve as a client
2. An EC2 Linux Server as a web server (This is where you will install WordPress)
3. An EC2 Linux server as a database (DB) server

## LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS “WEB SERVER”.

#### Step 1 — Prepare a Web Server

1. Launch an EC2 instance that will serve as “Web Server”. Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

<img width="1103" alt="Screenshot 2023-08-03 at 19 30 27" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/a4f86c35-58ad-41d8-b6cc-7015a94f69c1">

Attach all three volumes one by one to your Web Server EC2 instance.

<img width="1103" alt="Screenshot 2023-08-03 at 19 35 09" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/a4705a3b-f0c3-4eda-9aff-f2d8b823ea4c">

2. Open up the Linux terminal to begin configuration

3. Use lsblk command to inspect what block devices are attached to the server. Notice names of your newly created devices. All devices in Linux reside in /dev/ directory. Inspect it with ls /dev/ and make sure you see all 3 newly created block devices there – their names will likely be xvdf, xvdh, xvdg.

<img width="656" alt="Screenshot 2023-08-03 at 18 20 34" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/b8c17c66-a06b-4f15-9e7b-be61640db7c5">

4. Use df -h command to see all mounts and free space on your server

<img width="938" alt="Screenshot 2023-08-03 at 19 46 48" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/7966aaf8-e62f-4593-9132-c1c17d834c8d">

5. Use gdisk utility to create a single partition on each of the 3 disks: sudo gdisk /dev/xvdf use "n" to add a new partition.

<img width="666" alt="Screenshot 2023-08-03 at 19 50 35" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/e724c71d-c60e-4d4c-8955-4e07c9959a44">

6. Use lsblk utility to view the newly configured partition on each of the 3 disks.

<img width="379" alt="Screenshot 2023-08-03 at 19 53 30" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/02fe1726-5b82-49fc-80a2-103e36b2229c">

7. Install lvm2 package using sudo yum install lvm2. Run sudo lvmdiskscan command to check for available partitions.
Note: Previously, in Ubuntu we used apt command to install packages, in RedHat/CentOS a different package manager is used, so we shall use yum command instead.

Before installing the lvm2 package, run sudo yum update (to update the package) and also run sudo yum upgrade (to upgrade the packgae to the latest version). 

If you encounter an error message of "This system is not registered with an entitlement server. You can use subscription-manager to register". In your preferred editor (vi, vim, nano, etc.) open the “subscription-manager.conf” file using "sudo nano /etc/yum/pluginconf.d/subscription-manager.conf" . Inside the “subscription-manager.conf” file change enabled to 0 and save the file. Lastly issue the “yum clean all” command.

<img width="726" alt="Screenshot 2023-08-03 at 20 14 36" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/9d1e01d8-336c-44ee-8fa4-4558c01f902b">

Now, install lvm2 package using sudo yum install lvm2. Run sudo lvmdiskscan command to check for available partitions.

8. Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM, using "sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1".

9. Verify that your Physical volume has been created successfully by running sudo pvs.

<img width="362" alt="Screenshot 2023-08-03 at 20 19 15" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/6fb4ec7f-b81a-4765-9789-5afc6b8e4024">

10. Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg "sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1".

11. Verify that your VG has been created successfully by running sudo vgs.

<img width="720" alt="Screenshot 2023-08-03 at 20 22 17" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/2e34aa5c-955a-498b-b465-a6c21b4142c1">

12. Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs

sudo lvcreate -n apps-lv -L 14G webdata-vg

sudo lvcreate -n logs-lv -L 14G webdata-vg

14. Verify that your Logical Volume has been created successfully by running sudo lvs.

<img width="720" alt="Screenshot 2023-08-03 at 20 25 12" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/3411f5e5-019a-47c1-89ba-1a6807065872">

15. Verify the entire setup, using "sudo vgdisplay -v #view complete setup - VG, PV, and LV" and "sudo lsblk" to verify.

<img width="500" alt="Screenshot 2023-08-03 at 20 28 19" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/285d8dc7-2a4a-4d3e-b56a-0c63d0f03259">

16. Use mkfs.ext4 to format the logical volumes with ext4 filesystem.

sudo mkfs -t ext4 /dev/webdata-vg/apps-lv

sudo mkfs -t ext4 /dev/webdata-vg/logs-lv

<img width="640" alt="Screenshot 2023-08-03 at 20 30 11" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/a36846a1-f14e-4e48-b4de-f68f6f57976d">

17. Create /var/www/html directory to store website files.

sudo mkdir -p /var/www/html

18. Create /home/recovery/logs to store backup of log data

sudo mkdir -p /home/recovery/logs

19. Mount /var/www/html on apps-lv logical volume "sudo mount /dev/webdata-vg/apps-lv /var/www/html/"

20. Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system) "sudo rsync -av /var/log/. /home/recovery/logs/".

21. Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very important) "sudo mount /dev/webdata-vg/logs-lv /var/log".

22. Restore log files back into /var/log directory "sudo rsync -av /home/recovery/logs/log/. /var/log".

23. Update /etc/fstab file so that the mount configuration will persist after restart of the server.

The UUID of the device will be used to update the /etc/fstab file; Run "sudo blkid"

Update /etc/fstab in this format using your own UUID and rememeber to remove the leading and ending quotes.

<img width="677" alt="Screenshot 2023-07-28 at 14 49 50" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/ccaa4aec-0f7c-4814-836d-a85774629e1f">

24. Test the configuration, using "sudo mount -a" and reload the daemon, using "sudo systemctl daemon-reload".

25. Verify your setup by running df -h, output must look like this:

<img width="572" alt="Screenshot 2023-08-03 at 21 28 45" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/287ff5da-b4ba-4475-8d10-4c115d430f66">

#### Step 2 — Prepare the Database Server

Launch a second RedHat EC2 instance that will have a role – ‘DB Server’

Repeat the same steps as for the Web Server, but instead of apps-lv create db-lv and mount it to /db directory instead of /var/www/html/.

<img width="504" alt="Screenshot 2023-07-28 at 15 42 35" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/61c19734-a7aa-4515-bfc3-3f23538596e1">

#### Step 3 — Install WordPress on your Web Server EC2

1. Update the repository using "sudo yum -y update".

2. Install wget, Apache and it’s dependencies using "sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json".

3. Start Apache

sudo systemctl enable httpd

sudo systemctl start httpd

4. To install PHP and it’s depemdencies

sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

sudo yum module list php

sudo yum module reset php

sudo yum module enable php:remi-7.4

sudo yum install php php-opcache php-gd php-curl php-mysqlnd

sudo systemctl start php-fpm

sudo systemctl enable php-fpm

sudo setsebool -P httpd_execmem 1

5. Restart Apache using "sudo systemctl restart httpd"

6. Download wordpress and copy wordpress to var/www/html

mkdir wordpress
  
cd   wordpress
  
sudo wget http://wordpress.org/latest.tar.gz
  
sudo tar xzvf latest.tar.gz
  
sudo rm -rf latest.tar.gz
  
sudo cp wordpress/wp-config-sample.php wordpress/wp-config.php
  
sudo cp -R wordpress /var/www/html/

7. Configure SELinux Policies

sudo chown -R apache:apache /var/www/html/wordpress

sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R

sudo setsebool -P httpd_can_network_connect=1

#### Step 4 — Install MySQL on your DB Server EC2

sudo yum update

sudo yum install mysql-server

Verify that the service is up and running by using sudo systemctl status mysqld, if it is not running, restart the service using "sudo systemctl restart mysqld" and enable it using "sudo systemctl enable mysqld
"so it will be running even after reboot:

<img width="675" alt="Screenshot 2023-07-28 at 16 26 45" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/fd5b146e-b786-4f7d-89a8-42c0899bf81b">

#### Step 5 — Configure DB to work with WordPress

sudo mysql. You can also use "sudo mysql -u root -p" for already created mysql account.

CREATE DATABASE wordpress;

CREATE USER `myuser (wordpress)`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'the password created';

GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>';

FLUSH PRIVILEGES;

SHOW DATABASES;

exit

Use "sudo vi /etc/my.cnf" to bind address. Add the ip address used to set the mysql account.

<img width="575" alt="Screenshot 2023-07-28 at 22 09 58" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/6b386bdc-0448-46f6-bf3f-d1440d8158df">

Go back to the EC2 webserver and configure the PHP using "vi wp-config.php"

#### Step 6 — Configure WordPress to connect to remote database.

Hint: Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, you shall allow access to the DB server ONLY from your Web Server’s IP address, so in the Inbound Rule configuration specify source as /32

<img width="1053" alt="Screenshot 2023-08-04 at 00 44 30" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/79fd456a-4715-40e2-9f12-76ca282cbd52">

1. Install MySQL client and test that you can connect from your Web Server to your DB server by using mysql-client

sudo yum install mysql

sudo mysql -u username -p -h <DB-Server-Private-IP-address>

Verify if you can successfully execute SHOW DATABASES; command and see a list of existing databases.

<img width="636" alt="Screenshot 2023-08-04 at 00 42 30" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/163e66ee-d156-49a9-a726-4884a72a6ab5">




