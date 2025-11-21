Setup Steps:


PART 1:  Environment Setup 

-->Login to your AWS Console

-->Go to EC2 → Launch Instance

-->Choose Ubuntu Server (Free Tier eligible)

--> Select t2.micro

--> Create/Select a key pair

-->Open Port 22 (SSH) & 80 (HTTP) in Security Group

-->Launch the instance

2.  Connecting to the Instance via SSH

Obtain your instance public IP and run:

ssh -i  "devops-assignment.pem" ubuntu@ec2-16-171-134-104.eu-north-1.compute.amazonaws.com



3. Creating devops_intern User
sudo adduser devops_intern


Why: A separate user maintains secure access and avoids using root/ubuntu for tasks.

4. Granting Passwordless Sudo

Open sudoers file:

sudo visudo


devops_intern ALL=(ALL) NOPASSWD:ALL


5. Changing Hostname
sudo hostnamectl set-hostname sandeep-devops

<img width="1920" height="1080" alt="Screenshot (116)" src="https://github.com/user-attachments/assets/ab2e7b52-2971-47f8-9282-5d19be8a84c8" />



PART-2: Simple web service setup

1.install Nginx or Apache on the EC2 instance

-->sudo apt update
-->sudo apt install nginx -y

2. Create a simple HTML Page at /var/www/html/index.html displaying
  a)Your Name
  b)The instance id fetched from aws metadata
  c)the server uptime

  --> sudo nano /var/www/html/index.html

  -->curl http://169.254.169.254/latest/meta-data/instance-id

   -->uptime -p

   


<img width="1920" height="1080" alt="Screenshot (120)" src="https://github.com/user-attachments/assets/648b2a2f-32e9-4400-b41f-abad3ad34bb4" />


PART -3: Monitoring Script
1) Create a bash script /usr/local/bin/system_report.sh that prints the following metrics:
   
a)Current date and time 
b) Uptime 
c)CPU usage (%) 
d) Memory usage (%) 
e) Disk usage (%) 
f)Top 3 processes by CPU usage

-->sudo nano /usr/local/bin/system_report.sh
scripts include:
Date & Time

CPU Usage

Memory Usage

Disk Usage

Top 3 CPU Processes

-->sudo chmod +x /usr/local/bin/system_report.sh

2.Configure a cron job to run this script every 5 minutes and append the results to /var/log/system_report.log

-->crontab -e
-->*/5 * * * * /usr/local/bin/system_report.sh >> /var/log/system_report.log




<img width="1920" height="1080" alt="Screenshot (121)" src="https://github.com/user-attachments/assets/3203ac74-34a8-429f-9b41-c7c780742a49" />



<img width="1920" height="1080" alt="Screenshot (122)" src="https://github.com/user-attachments/assets/2404e7d4-d8b2-44ff-bd83-a6f63493b1e3" />



PART-4:AWS Integration:

1) create a cloudwatch log group named /devops/intern-metrics
2) use the AWS CLI to push the system_report.log file to this CloudWatch log group


-->aws logs create-log-group --sandeep_devops /devops/intern-metrics

-->aws logs put-log-events \
  --log-group-name /devops/intern-metrics \
  --log-stream-name system-report \
  --log-events file://log_events.json




--><img width="1920" height="1080" alt="Screenshot (123)" src="https://github.com/user-attachments/assets/bcc6309e-0fe6-4182-9e21-a709c88a056f" />


<img width="1920" height="1080" alt="Screenshot (124)" src="https://github.com/user-attachments/assets/6cd5e366-f3f3-464d-80cd-ca99f27214a6" />




<img width="1920" height="1080" alt="Screenshot (125)" src="https://github.com/user-attachments/assets/aa3e400c-9c9f-4291-a490-0c7a878954a4" />



<img width="1920" height="1080" alt="Screenshot (126)" src="https://github.com/user-attachments/assets/097144d3-044b-41d7-a4a1-5edb85a5748e" />




