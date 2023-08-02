# CLIENT-SERVER ARCHITECTURE WITH MYSQL

Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another.

In their communication, each machine has its own role: the machine sending requests is usually referred as “Client” and the machine responding (serving) is called “Server”.

#### IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS).

To demonstrate a basic client-server using MySQL Relational Database Management System (RDBMS), follow the below instructions

1. Create and configure two Linux-based virtual servers (EC2 instances in AWS).

Server A name - `mysql server`

Server B name - `mysql client`



<img width="1022" alt="Screenshot 2023-07-18 at 13 49 02" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/36ac4cf2-8640-49ae-8181-3e72ad13c303">


2. On mysql server Linux Server install MySQL Server software.

<img width="583" alt="Screenshot 2023-07-18 at 13 53 35" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/7be56d56-577a-45ad-b03c-0ca8e05338c8">

Mysql server installed and connected.

3. On mysql client Linux Server install MySQL Client software.

<img width="583" alt="Screenshot 2023-07-18 at 13 57 07" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/c47d81b0-9a37-477e-84ed-31a71e8e5414">

Mysql client installed

4. By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.

<img width="1000" alt="Screenshot 2023-07-18 at 14 01 29" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/f88af2a7-4911-47e9-9125-e90427e4f54e">



5.  You need to configure MySQL server to allow connections from remote hosts using: sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

   Replace ‘127.0.0.1’ to ‘0.0.0.0’ like this:

   <img width="743" alt="Screenshot 2023-08-02 at 17 10 15" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/5b42858d-d6e9-4e4a-8330-47fd0534d2cf">

This is what you get;


<img width="586" alt="Screenshot 2023-07-18 at 14 16 56" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/f3b83d27-036d-4506-aab6-654c75939881">


Create User name, Database and grant all privileges using 'sudo mysql'


<img width="706" alt="Screenshot 2023-07-18 at 14 27 30" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/ed8293ea-6553-4663-a8f2-90968c75b99c">


6. From mysql client Linux Server connect remotely to mysql server Database Engine without using SSH. You must use the mysql utility to perform this action.


<img width="635" alt="Screenshot 2023-07-18 at 14 39 23" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/3c32c258-1639-4b8c-87be-3bf144f1c3a6">


7. Check that you have successfully connected to a remote MySQL server and can perform SQL queries, using ‘Show databases;’

<img width="635" alt="Screenshot 2023-07-18 at 14 41 52" src="https://github.com/LarryLeo9/Darey.io/assets/136237391/a14cd6e5-97e2-413b-b2b4-f8a2cd73d5ca">




Thank you.





