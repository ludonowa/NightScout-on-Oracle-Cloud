# NightScout-on-Oracle-Cloud
create a free oracle linux vm For NightScout instance

Create the Linux Oracle Cloud VM

Oracle Cloud account creation

Connect to a https://cloud.oracle.com
![image](https://user-images.githubusercontent.com/96974624/197184979-70a2cc47-9ad0-44d1-a18d-90699ef658f6.png)
 
Click on Sign Up
![image](https://user-images.githubusercontent.com/96974624/197185008-713136af-4a80-4c69-82b7-6ebc8f0fe4f4.png)
 
Enter the information and check “i m an human” and then click on verify my email
In the email received from Oracle Cloud noreply@verify.signup.us-ashburn-1.oci.oraclecloud.com click on “verify email”
![image](https://user-images.githubusercontent.com/96974624/197185049-8980f6ad-1027-4e9f-abbd-76a4a080aa60.png)
 
![image](https://user-images.githubusercontent.com/96974624/197185143-1039db12-9377-49b8-98be-a3016f1da3e7.png)

 
Finish completing the form with all the other information
-	password
-	Choose individual
-	keep the “Cloud account name” it will be needed to logon 
 

Click on “Continue”
-	enter your address
-	your phone number
 











Then enter your Banking card information. it’s mandatory but if you follow this tutorial the VM will be free of charge

 

Check the checkbox and click on start your free period
Virtual Machine creation

From the Dashboard search « Launch Resource » and then click on « Create a VM instance »
 

Edit « Image and Shape »
Keep Oracle linux 8 en OS
 
Click on change shape
Select Ampere Arm-based processor
 






with this type of processor you can choose 4 OCPU and 24 GB of Memory
 
Verify that “Assign a public IPV4 address” is YES

 
Sur Oracle Cloud la seul manière de se connecter sur la machine virtuel est avec une paire de clef
 
Save the private and the public keys on your computer
 
Then Click to Create
After some minutes the virtuel machine will be up and running


 

Get the public IP address

Oracle Cloud VCN configuration (firewall)

Click on burger at the top left
 
Click on “Network » on he left pane and then on “Virtual Cloud Networks” (VCN)
Click on the VCN available 
Click on public subnet
 
Click on security list
 

Click on add ingress rules
 
We will add the http access TCP Port 80
Complete the form like the example bellow
 
Click on Save Change
We will add https access TCP Port 443
Click on add ingress rules
Complete the form like the example bellow


 

Your Oracle Cloud environment will permit http and https acsess

MongoDB Atlas Database Creation
Database limitations
We will use a cluster M0 free of charge for the nightscout Atlas MongoDB database, the size is limited to 512MB
You will maybe need to do some cleaning, be sure that dbsize has been added in enable parameter to have a look on the space used. you can increase the size with a shared cluster M2, the prize is 9$/month
MongoDB Atlas account creation
In your Browser open a new tab and enter the url bellow
https://www.mongodb.com/cloud/atlas/register
Enter your information in the form and click to  





























You will receive an email from MongoDB Atlas to check your email address, check your Spam folder
 

In the email received from MongoDB Cloud mongodb-account@mongodb.com click on  
 


A new tab will be open asking you to confirm your email address. Click on  
 
 

Complete the form with the information like the example bellow and click on “Finish”.
 

 
Database Creation

Click on “Build a database”
 
Select “Create a cluster in Shared Clusters (FREE)”

 

If banking information are request, stop the procedure

We will create a free of charge cluster: no banking information needed.
 

Check that you have selected Shared, keep the default access and click on Create Cluster
 


Select Username and Password and invent a database username (for example nightscout) and a database password (keep the password safe from other).

Database Password recommendation:
- do not use the same as your Atlas account.
- do not use special character and space. 
- use only letters and numbers.
 
Click on « Create User »
Select « My Local Environment » put in the field « My IP address »  0.0.0.0/0 (i twill permit to all IP address to access to your database if the server use for nightscout got a fixe IP address you can enter the IP of the server. This is the case of the oracle cloud VM created before, then you can put the IP of the Oracle VM

 

Then click “Add Entry »

Click on “Finish and Close”
 

Click on “Go to Databases”
 

Atlas will create your default cluster, it can take more than 3 minutes
 

Click on “Connect”
 

Select “Connect your Application”
 
Keep Node.js driver and version
Click on Copy and keep the Database connection string 
 







Backup your database⌁
Not a normal operation
Making a backup of your database is either a good idea or necessary if you want to migrate it to another database.
This is not an easy operation and requires command line instructions using a computer.
Install the database tools⌁
Follow this link to install the CLI tool on your computer.
Dump your database⌁
Get your MONGODB_URI handy to find the missing pieces (password and database name).
Log in your MongoDB account https://cloud.mongodb.com/.
1.	Select your organization
2.	Select your database
3.	In the advanced options menu, select Command line tools
 

Scroll down to Binary import and Export tools, copy the mongodump command line.
 
Paste the command line in a text editor.
Look into your MONGODB_URI and replace <PASSWORD> with your database password and entually (if you have one) <DATABASE> with your database name.
 

Open a command line utility (CMD, Terminal, ...) and make your way to the utility folder (if you don't want to include it in your system path).
For example in Windows 64bits it's in C:\Program Files\MongoDB\Tools\100\bin.
Copy and paste your mongodump command, run it.
 

You will find the database dump in a subfolder called dump with your database name in a subfolder.
 


 
Connect to Oracle VM using Putty

Putty is a free ssh tool available here Download PuTTY: latest release (0.77) (greenend.org.uk)

Convert the private Key

Launch puttyGen
 
Open the provate key save during the Oracle Cloud VM creation
when the Private Key is imported you can click on save Private Key with a .ppk extension

Putty configuration
 
Launch putty
 
In the field Hostname, enter the public IP of the Oracle Cloud VM

  
Click on Browse et select the key saved previously
 
In “Auto-login username” add opc
 
Give a name to your session and click on Save Button

Now you can click to open
 
accept the Finger print server
 
You are now connected to your VM
 
Create a public DNS Entry
To create a free public DNS entry you can use Free dynamic DNS service | Dynu Systems, Inc.
 
Click on “Sign Up”
 
In option 1 add your hostname and select a domain
Click Add
Complete the form with your information
 
Click on Submit and validate your email address
Complete the DNS entry with the public IP of oracle Cloud VM
 
Click to save

Nightscout installation

This tutorial explain own to install Nightscout on CentOS 8, Oracle Linux 8 ou Redhat8. We will suppose that you are familiare with bash command on linux.
To Resume:
We have a server with CentOS 8, RedHat8 or Oracle lunux 8
Nous avons un serveur installé exécutant CentOS 8, accès via ssh. SELinux configuration will explain later. We will need to be connected as root. The server should have a Staic/ fixe IP adddress. A public DNS entry should be exist for the Fixe IP address and will be explain later. During the installation, I will use "night.freeddns.org".

Start to be root
sudo –s
 
dnf install git nano mc -y
dnf groupinstall 'Development Tools' -y


NodeJS Installation
The installation will be done from the AppStream repository, let check the existing version
dnf module list nodejs
Cela devrait ressembler à ceci :
Oracle Linux 8 Application Stream (aarch64)
Name      Stream    Profiles                                Summary
nodejs    10 [d]    common [d], development, minimal, s2i   Javascript runtime
nodejs    12        common [d], development, minimal, s2i   Javascript runtime
nodejs    14        common [d], development, minimal, s2i   Javascript runtime
nodejs    16        common [d], development, minimal, s2i   Javascript runtime

Two flows are avalable, 10 et 12. [d] show that the revision 10 is by default. we need to move to the 12.
dnf module enable nodejs:12 -y
if the revision 10 flow is used launch a reset on the revision 10 flow and relaunch the command
dnf module reset nodejs:10 -y 
dnf module enable nodejs:12 -y
NodeJS installation
dnf install nodejs –y
dnf install npm -y
Now NodeJS and npm are install let check the revision
node --version && npm --version
I have: v12.18.3, 6.14.6, revision can be different but the revision should not be less than mine

Nightscout Deployment
NightScout cannot run under root, so we will create a user a dedicated user with home folder on /opt/nightscout
useradd -d /opt/nightscout -m -c "User for nightscout" nightscout
Connect under nightscout user and install NightScout
su - nightscout
git clone https://github.com/nightscout/cgm-remote-monitor.git
cd cgm-remote-monitor/
npm install
Create an executable file on /opt/nightscout/cgm-remote-monitor/start.sh with the following content. Take care: MONGO_CONNECTION – parameter to connect to your Atla MongoDB create before. API_SECRET – secret key need to access to NightScout website. Some other option can be find in this chapter
vi /opt/nightscout/cgm-remote-monitor/start.sh

#!/bin/bash

# environment variables
export MONGO_CONNECTION="mongodb://userdb:passdb@localhost:27017/nightscout"
export DISPLAY_UNITS="mg/dL"
export BASE_URL="http://night.freeddns.org"
export PORT=1337
export DEVICESTATUS_ADVANCED=true
export INSECURE_USE_HTTP=true
export mongo_collection="entries"
export API_SECRET="1234567890XY"
export ENABLE="careportal basal rawbg cob iob cage bwp upbat sage pump"
export TIME_FORMAT=24
export THEME=colors
export LANGUAGE=fr
export SCALE_Y=linear
export HOSTNAME=0.0.0.0

# start server
node /opt/nightscout/cgm-remote-monitor/server.js
Let's make our file executable
chmod +x /opt/nightscout/cgm-remote-monitor/start.sh
Launch start.sh to start NightScout.
./start.sh
Something like this appears in the console
Load Complete:

data loaded: reloading sandbox data and updating plugins
For the COB plugin to function you need a treatment profile
For the Basal plugin to function you need a treatment profile
WS: emitted clear_alarm to all clients
tick 2022-10-12T16:52:27.753Z

stop the script with Ctrl+C
We do other work under the root user, press Ctrl + D and return to the root console. We are making a service for Nightscout to start automatically, create a file /etc/systemd/system/nightscout.service with the following content.
vi /etc/systemd/system/nightscout.service

[Unit]
Description=Nightscout Service
After=network.target

[Service]
Type=simple

User=nightscout
Group=nightscout

WorkingDirectory=/opt/nightscout/cgm-remote-monitor
ExecStart=/opt/nightscout/cgm-remote-monitor/start.sh

[Install]
WantedBy=multi-user.target

Reload the daemon and start the service
systemctl daemon-reload
systemctl enable nightscout.service
systemctl start nightscout.service
After a while, we check that everything is working
systemctl status nightscout.service


● nightscout.service - Nightscout Service
   Loaded: loaded (/etc/systemd/system/nightscout.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2020-09-07 22:45:25 +04; 18s ago
 Main PID: 37456 (start.sh)
    Tasks: 12 (limit: 12525)
   Memory: 61.0M
   CGroup: /system.slice/nightscout.service
           ├─37456 /bin/bash /opt/nightscout/cgm-remote-monitor/start.sh
           └─37458 node /opt/nightscout/cgm-remote-monitor/server.js


Verify that the port is listening and verify that website reply correctly
netstat -ltupen | grep 1337

tcp        0      0 0.0.0.0:1337            0.0.0.0:*               LISTEN      1001       880135     318471/node

curl http://localhost:1337 | grep "<title>"

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 42973  100 42973    0      <title>Nightscout</title>
 0  2622k      0 --:--:-- --:--:-- --:--:-- 2622k

If you see something like this, the site is working
Nginx installation
Install nginx and certbot (to get a certificate from Let's Encrypt)
Let us start by updating the operating system
dnf update -y 
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y


dnf install nginx certbot python3-certbot-nginx -y

Nginx Configuration
Let's open the /etc/nginx/nginx.conf file and comment out the server section, i.e. bring it to this form
vi /etc/nginx/nginx.conf

#    server {
#        listen       80 default_server;
#        listen       [::]:80 default_server;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        location / {
#        }
#
#        error_page 404 /404.html;
#            location = /40x.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#            location = /50x.html {
#        }
#    }

Create a new directory and configuration files
mkdir /etc/nginx/includes
In /etc/nginx/includes/ssl
vi /etc/nginx/includes/ssl

#ssl_certificate	/etc/pki/tls/certs/fullchain.pem;
#ssl_certificate_key	/etc/pki/tls/certs/privkey.pem;

ssl_protocols TLSv1.2 TLSv1.3;

ssl_session_timeout 1d;
ssl_session_cache shared:SSL:50m;
ssl_session_tickets off;

ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
ssl_prefer_server_ciphers on;

# HSTS (ngx_http_headers_module is required) (15768000 seconds = 6 months)
add_header Strict-Transport-Security "max-age=15768000; includeSubdomains; ";

# OCSP Stapling ---
# fetch OCSP records from URL in ssl_certificate and cache them
ssl_stapling on;
ssl_stapling_verify on;

ssl_trusted_certificate /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem;

resolver 8.8.8.8 valid=300s;
resolver_timeout 5s;


In /etc/nginx/includes/proxy_pass_reverse
vi /etc/nginx/includes/proxy_pass_reverse

proxy_set_header X-Forwarded-Host $host;
proxy_set_header X-Forwarded-Server $host;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;


Let us create a file with the configuration of our site /etc/nginx/conf.d/night.conf
vi /etc/nginx/conf.d/night.conf

server {
    listen  80;
    server_name night.freeddns.org default_server;
    # enforce https
    return 301 https://$server_name$request_uri;
}
server {
    listen 443 ssl http2;
    server_name night.freeddns.org default_server;

    access_log /var/log/nginx/night-access.log;
    error_log /var/log/nginx/night-error.log;

    include /etc/nginx/includes/ssl;

    location / {
        proxy_pass http://127.0.0.1:1337/;
        include /etc/nginx/includes/proxy_pass_reverse;
    }
}


To obtain a certificate you must modify the nginx settings, for this we will create a directory
mkdir -p /var/www/letsencrypt
chown -R nginx:nginx /var/www/letsencrypt


We now need to ensure that any request such as: http://night.freeddns.org/.well-known/acme-challenge leads to the physical location /var/www/letsencrypt/.well-known/acme-challenge , To do this, create a /etc/nginx/includes/letsencrypt file with the following content
vi /etc/nginx/includes/letsencrypt

location ^~ /.well-known/acme-challenge/ {
   default_type "text/plain";
   root /var/www/letsencrypt;
}
location = /.well-known/acme-challenge/ {
   return 404;
}

Now let's make changes to the /etc/nginx/conf.d/night.conf file by adding the line include /etc/nginx/includes/letsencrypt; it should look like this: (I cut the extra lines)
vi /etc/nginx/conf.d/night.conf

server {
    listen 443 ssl http2;
..........
    include /etc/nginx/includes/ssl;
    include /etc/nginx/includes/letsencrypt; 
..........
}

You can now request the issuance of certificates. I draw your attention to the fact that you must have a correctly configured entry in the DNS, in which all requests with the name night.freeddns.org must reach your server. You can check this with the command, where 123.45.67.89 is your server's IP address. In addition, you need to open ports 80, 443 on the firewall.
host night.freeddns.org
night.freeddns.org has address 123.45.67.89


Configuration de firewalld
Par défaut sur Oracle c’est firewalld qui est activé, commencons par verifier le status de firewalld
systemctl status firewalld.service

● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor p>
   Active: active (running) since Mon 2022-10-10 18:36:45 GMT; 2 days ago
     Docs: man:firewalld(1)
 Main PID: 1495 (firewalld)
    Tasks: 2 (limit: 9097)
   Memory: 65.3M
   CGroup: /system.slice/firewalld.service
           └─1495 /usr/libexec/platform-python -s /usr/sbin/firewalld --nofork >



firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --zone=public --add-service=https --permanent
firewall-cmd  --reload


Configuration de SELinux
Let's add the http https and 1337 ports to selinux
semanage permissive -a httpd_t
semanage port -a -t http_port_t -p tcp 1337


Check that the command has been taken into account
semanage port -l | grep http_port_t

http_port_t                    tcp      1337,80, 81, 443, 488, 8008, 8009, 8443, 9000
pegasus_http_port_t            tcp      5988

Website certificates creation
You can now request to create certificates
# Request without email notification
certbot certonly --nginx -d night.freeddns.org --register-unsafely-without-email
# OR this one, with a notification to my_email@domain.fr
certbot certonly --nginx -d night.freeddns.org -m my_email@domain.fr


As a result, you should see the following (trimmed) log
.........
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/night.freeddns.org/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/night.freeddns.org/privkey.pem
.........


Here we are interested in the path to the certificate and the private key. Let's modify the /etc/nginx/includes/ssl file, at the very beginning
vi /etc/nginx/includes/ssl

ssl_certificate		/etc/letsencrypt/live/night.freeddns.org/fullchain.pem;
ssl_certificate_key	/etc/letsencrypt/live/night.freeddns.org/privkey.pem;


Now let's configure nginx services and run it
systemctl enable nginx.service
systemctl restart nginx.service

You can now navigate to the https://night.freeddns.org site and continue setting up the site itself. You can also verify the correct configuration of nginx and certificates on this site.
Automatic certificates renewal
We will create a daily task to verify and renew certificates if needed
vi /etc/cron.daily/letsencrypt
Add the following text, to randomly request a renewal
#!/bin/sh
python3 -c 'import random; import time; time.sleep(random.random() * 3600)' && certbot renew

