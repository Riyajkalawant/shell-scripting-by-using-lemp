
#  LEMP Stack Setup (Ubuntu 24.04 / AWS EC2)

This guide explains how to install and configure a **LEMP Stack** — **Linux, Nginx, MySQL, PHP (8.3)** — on an **Ubuntu EC2 instance** using a simple automation script `lemp.sh`.

---

##  Components Installed

| Component | Description |
|------------|-------------|
| **Linux (Ubuntu 24.04 LTS)** | Base operating system |
| **Nginx** | Web server |
| **MySQL Server** | Database server |
| **PHP 8.3 + FPM** | PHP runtime and FastCGI handler |

---

##  What the Script Does

The script performs the following:

1. Updates apt and upgrades packages.  
2. Installs **Nginx**, **MySQL Server**, and **PHP 8.3 (with FPM)**.  
3. Starts and enables all services.  
4. Creates a sample web page (`index.php`) showing PHP configuration.  
5. Tests the local web server using `curl`.

---

##  Script: `lemp.sh`

```bash
#!/bin/bash
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install nginx -y
sudo apt-get install mysql-server -y
sudo apt-get install php8.3 -y
sudo apt-get install php8.3-fpm -y
sudo service nginx start
sudo service mysql start
sudo service php8.3-fpm start
cd /var/www/html
sudo rm index.html
sudo touch index.html index.php
echo "king sk" | sudo tee index.html
echo "<?php phpinfo(); ?>" | sudo tee index.php
curl http://localhost
curl http://localhost/index.php
```

---

##  Steps to Run

### 1. Connect to your EC2 instance
```bash
ssh -i your-key.pem ubuntu@<your-ec2-public-ip>
```

### 2. Create and open the script
```bash
nano lemp.sh
```

### 3. Paste the above code and save  
Press **Ctrl + O**, then **Enter**, and exit with **Ctrl + X**.

### 4. Make the script executable
```bash
chmod +x lemp.sh
```

### 5. Run the script
```bash
./lemp.sh
```

---

##  Example Output (Script Execution)

![Script execution in terminal](./img/Screenshot%202025-09-15%20002602.png)

> *All required packages (nginx, mysql-server, php8.3, php8.3-fpm) get installed and services start automatically.*

---

##  Verify PHP Setup

After the script completes, open your browser and visit:

```
http://<your-ec2-public-ip>/index.php
```

If everything is correct, you’ll see the **PHP info page**:

![PHP info page showing PHP 8.3.6 on Ubuntu](./img/Screenshot%202025-09-15%20002239.png)

> This confirms that **PHP-FPM** and **Nginx** are properly integrated.

---

##  Optional Security and Configuration Steps

### 1. Secure MySQL
```bash
sudo mysql_secure_installation
```

### 2. Enable Firewall
```bash
sudo ufw allow 'Nginx Full'
sudo ufw enable
sudo ufw status
```

### 3. Enable HTTPS with Let's Encrypt
```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx
```

---

##  File & Directory Info

| File/Directory | Purpose |
|----------------|----------|
| `/var/www/html/index.php` | PHP test file |
| `/etc/nginx/sites-available/default` | Default Nginx config |
| `/var/log/nginx/error.log` | Error logs |
| `/run/php/php8.3-fpm.sock` | PHP-FPM socket |

---

##  Tutorial Screenshots

###  PHP 8.3.6 Verification  
![phpinfo result](./img/Screenshot%202025-09-15%20002239.png)

###  Script File in Nano Editor  
![lemp.sh opened in nano](./img/Screenshot%202025-09-15%20002352.png)

###  Script Running in Terminal  
![terminal execution](./img/Screenshot%202025-09-15%20002602.png)

###  html output 
![html result](./img/Screenshot%202025-09-15%20002632.png)


---

##  Notes

- Tested on **Ubuntu 24.04 LTS** running on **AWS EC2 (t2.micro)**.  
- Make sure **port 80 (HTTP)** and **port 443 (HTTPS)** are open in your AWS Security Group.  
- For production, remove or secure `phpinfo()` — it exposes sensitive system information.

---

##  Author

**Riyaj Jamir Kalawant**  
Email :-*riyajkalawant8@gmail.com*  
=======

