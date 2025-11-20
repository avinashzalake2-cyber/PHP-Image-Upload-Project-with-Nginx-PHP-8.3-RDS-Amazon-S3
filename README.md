# 🚀 PHP Image Upload Project with Nginx, PHP 8.3, RDS & Amazon S3  
A **fully production-ready Cloud & DevOps project** showcasing a real-world deployment using:

- **PHP 8.3 + Nginx**
- **Amazon RDS (MySQL)**
- **Amazon S3 for Image Storage**
- **IAM Roles for Secure Access**
- **Linux & Ubuntu Server Administration**
- **AWS Deployment Workflow**
- **Composer + AWS SDK**
- **CI/CD Ready Structure**
- **Architecture Diagrams**
- **Troubleshooting**
- **Screenshots Section**

---

## 🧭 Project Overview  
This project uploads an image using a **PHP form** hosted on **Nginx** running on an **Ubuntu EC2 instance**.  
The workflow:

1. User uploads the image  
2. PHP saves it temporarily  
3. It is then uploaded to **Amazon S3**  
4. Metadata (name, file URL) is stored inside **Amazon RDS (MySQL)**  

➡️ A clean, scalable, cloud-native design used in real production systems.

---

## 🎯 Objectives
- ✔ Learn PHP 8.3 with Nginx  
- ✔ Implement secure file uploads to S3  
- ✔ Save metadata inside RDS MySQL  
- ✔ Configure IAM role for EC2 → S3 access  
- ✔ Deploy a production-ready cloud architecture  
- ✔ Improve resume with a real AWS project  

---

## 🧱 Architecture Diagram

User → Nginx → PHP 8.3 → RDS (MySQL)

→ Amazon S3 Bucket



---

## 🛠️ Technologies Used
- AWS EC2  
- AWS S3  
- AWS RDS  
- AWS IAM  
- PHP 8.3  
- Nginx  
- Ubuntu Linux  
- Composer  
- AWS SDK for PHP  

---

## 🧰 Prerequisites
- AWS Account  
- EC2 Ubuntu Instance  
- IAM Role with S3 permissions  
- RDS MySQL Instance  
- S3 Bucket  
- PHP 8.3 + Required Extensions  
- Composer Installed  

---

# 📌 1. Install Required Packages
```bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install nginx -y
sudo apt install php8.3 php8.3-fpm php8.3-mysql -y
sudo systemctl enable nginx
sudo systemctl start nginx
```
📌 2. Install & Configure MariaDB / MySQL
```
sudo apt install mariadb-server -y
sudo systemctl start mariadb
sudo systemctl enable mariadb
```
Create Database
```
sudo mysql -u admin -p -h <RDS-ENDPOINT>
CREATE DATABASE facebook;
USE facebook;

CREATE TABLE posts (
  pid INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255),
  url VARCHAR(500)
);
```
📌 3. Configure Nginx for PHP
```
sudo nano /etc/nginx/sites-available/default
Replace with:
nginx
Copy code
server {
    listen 80;
    server_name _;

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
    }
}
```
Restart Nginx:
```
sudo systemctl restart nginx
```
📌 4. Create Project Structure
```
/var/www/html/
 ├── form.html
 ├── upload.php
 └── uploads/

```
Create uploads folder:
```
sudo mkdir /var/www/html/uploads
sudo chmod 777 /var/www/html/uploads
```
📌 5. Install Composer & AWS SDK
```
sudo curl -sS https://getcomposer.org/installer | sudo php
sudo mv composer.phar /usr/local/bin/composer
sudo ln -s /usr/local/bin/composer /usr/bin/composer
sudo composer require aws/aws-sdk-php
```
📌 6. Configure IAM Role for EC2 → S3 Access

Step 1 — Create IAM Role
AWS Console → IAM → Roles

Create Role → EC2

Attach: AmazonS3FullAccess

Name: EC2-S3-Access-Role

Step 2 — Attach Role
EC2 → Actions → Security → Modify IAM Role → Select EC2-S3-Access-Role

📌 7. Create Files & Add Code

form.html
```
<!DOCTYPE html>
<html>
<body>
  <h2>Upload Image</h2>
  <form action="upload.php" method="POST" enctype="multipart/form-data">
      <input type="text" name="name" placeholder="Enter name"><br><br>
      <input type="file" name="file"><br><br>
      <button type="submit">Upload</button>
  </form>
</body>
</html>
```
upload.php
```

<?php
require 'vendor/autoload.php';
use Aws\S3\S3Client;

$name = $_POST['name'];
$file = $_FILES['file']['name'];
$tmp = $_FILES['file']['tmp_name'];

move_uploaded_file($tmp, "uploads/".$file);

$s3 = new S3Client([
    'region'  => 'ap-south-1',
    'version' => 'latest'
]);

$bucket = "your-bucket-name";
$key = basename($file);

$result = $s3->putObject([
    'Bucket' => $bucket,
    'Key'    => $key,
    'SourceFile' => "uploads/".$file,
    'ACL' => 'public-read'
]);

$url = $result['ObjectURL'];

$mysqli = new mysqli("rds-endpoint", "admin", "password", "facebook");
$query = "INSERT INTO posts(name, url) VALUES('$name', '$url')";
$mysqli->query($query);

echo "Uploaded Successfully! <br> Image URL: $url";
?>
```
📌 8. Test the Application
Open browser:

arduino
Copy code
http://YOUR-EC2-PUBLIC-IP/form.html
🌥️ AWS Deployment Checklist
✔ Create EC2

✔ Install Nginx + PHP

✔ Create S3 bucket

✔ Create RDS MySQL

✔ Attach IAM Role

✔ Upload project files

✔ Restart services

🔐 Security Best Practices

Use IAM roles, not access keys

Restrict Security Groups

Enable HTTPS

Disable public access to RDS

📸 Recommended Screenshots

EC2 Dashboard

Nginx Running

RDS Configuration

S3 Bucket

Application Output

Project Directory

🧪 Testing

Upload valid images

Test large files

Test invalid formats

Check RDS table entries

🧹 Troubleshooting

Issue	Fix
404 Error	Check Nginx root path
RDS Timeout	Update SG inbound rules
S3 Upload Fail	Check IAM Role Permissions
PHP Errors	Enable error logs
Permission Denied	Fix folder permissions

👨‍💻 Author

Avinash Zalake
Cloud & DevOps Engineer

AWS | Linux | PHP | RDS | S3 | CI/CD
- [LinkedIn Profile](https://www.linkedin.com/in/%F0%9D%99%B0%F0%9D%9A%9F%F0%9D%9A%92%F0%9D%9A%97%F0%9D%9A%8A%F0%9D%9A%9C%F0%9D%9A%91-%F0%9D%9A%89%F0%9D%9A%8A%F0%9D%9A%95%F0%9D%9A%94%F0%9D%9A%8E-1884b024a?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app)
