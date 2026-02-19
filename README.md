📘 Lab 5 — Assignment 1
=======================

Deploy and Connect AWS RDS Database to EC2 Application (Orchard Core CMS)
-------------------------------------------------------------------------

🚀 Project Overview
-------------------

This project demonstrates deployment of an application on AWS EC2 and connecting it to an AWS RDS PostgreSQL database. The application (Orchard Core CMS) performs all CRUD operations using the deployed database.

🏗️ Architecture
----------------

`   User → Internet → Nginx → Orchard Core CMS (EC2) → AWS RDS PostgreSQL   `

☁️ Services Used
----------------

*   AWS EC2 (Ubuntu 22.04 LTS)
    
*   AWS RDS (PostgreSQL)
    
*   Nginx (Reverse Proxy)
    
*   Orchard Core CMS (.NET 8)
    
*   Security Groups (Network control)
    

🖥️ EC2 Setup (Application Deployment)
--------------------------------------

1.  Launched an EC2 instance (t3.micro, Ubuntu 22.04).
    
2.  Installed required packages:
    

`   sudo apt update  sudo apt install nginx -y   `

1.  Installed .NET 8 SDK.
    
2.  Created Orchard Core CMS project using official template.
    
3.  Published application to:
    

`   /var/www/orchard   `

1.  Configured systemd service to run the application on port 5000.
    
2.  Configured Nginx as reverse proxy to forward HTTP traffic to the app.
    

🗄️ AWS RDS PostgreSQL Deployment
---------------------------------

1.  Open AWS Console → RDS → Create Database.
    
2.  Selected:
    

*   Engine: PostgreSQL
    
*   Instance class: db.t4g.micro (Free Tier)
    
*   Public access: Yes
    
*   Initial DB name: orcharddb
    
*   Master username: adminvish
    

1.  Created database instance and waited until status = Available.
    

🔐 Security Configuration
-------------------------

### EC2 Security Group

Allowed inbound rules:

*   HTTP (80) — Anywhere
    
*   SSH (22) — My IP
    

### RDS Security Group

Configured inbound rule to allow database access only from EC2 security group:

`   Type: PostgreSQL  Port: 5432  Source: EC2 Security Group (launch-wizard-1)   `

No public access allowed from the internet.

🔗 Database Connection from EC2
-------------------------------

1.  Connected to EC2 via SSH:
    

`   ssh -i orchard-key.pem ubuntu@   `

1.  Installed PostgreSQL client:
    

`   sudo apt install postgresql-client -y   `

1.  Connected to RDS database:
    

`   psql -h  -U adminvish -d postgres -p 5432   `

1.  Created application database:
    
`   CREATE DATABASE orcharddb;   `

⚙️ Application Configuration
----------------------------

1.  Reset Orchard Core setup to remove SQLite configuration.
    
2.  Opened application in browser:
    

`   http://   `

1.  Selected database type: PostgreSQL.
    
2.  Provided RDS connection string:
    
`   Server=;Port=5432;Database=orcharddb;User Id=adminvish;Password=;   `

1.  Completed setup.
    

Orchard automatically created tables in RDS.

🔄 CRUD Operations Demonstrated
-------------------------------

CRUD operations were performed through the application using user management:

### ✔ Create

Added a new user in Orchard admin panel.

### ✔ Read

Viewed the user list.

### ✔ Update

Modified user details.

### ✔ Delete

Removed the user.

All operations affected the AWS RDS PostgreSQL database.

🌐 Application Access
---------------------

Application URL:

`   http:// <EC2_PUBLIC_IP>  `

Admin panel:

`   http://<EC2_PUBLIC_IP>/admin   `

📊 Verification of Database Usage
---------------------------------

Verified database connectivity by querying RDS from EC2:

`   psql -h  -U adminvish -d orcharddb -p 5432   `

Checked tables:

`   \dt   `

✅ Conclusion
------------

The EC2-hosted application was successfully connected to AWS RDS PostgreSQL, and all CRUD operations were performed using the deployed cloud database, fulfilling the assignment requirements.
