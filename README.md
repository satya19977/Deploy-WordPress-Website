# Deploy-WordPress-Website-The-Traditional-Way
Resources Used - **VPC(NAT Gateway,IGW,Route Table) , Route 53, RDS,EC2,ALB, EFS**
# Architecture Of WordPress Website
![image](https://github.com/satya19977/Deploy-WordPress-Website-The-Traditional-Way/assets/108000447/cefd25ee-0874-44c5-b959-be75ea2a8baf)

# Output
![image](https://github.com/satya19977/Deploy-WordPress-Website-The-Traditional-Way/assets/108000447/1c06443a-732c-47c1-8510-7de67a3db438)
![image](https://github.com/satya19977/Deploy-WordPress-Website-The-Traditional-Way/assets/108000447/9459884e-37d7-4835-b3ba-d8c36e29d244)
![image](https://github.com/satya19977/Deploy-WordPress-Website-The-Traditional-Way/assets/108000447/df9307f1-a123-4915-9f82-1d30b80125f7)
![image](https://github.com/satya19977/Deploy-WordPress-Website-The-Traditional-Way/assets/108000447/618b0d44-9ce4-472c-ae66-5db24debfdb7)


## VPC
### Internet Gateway
### Subnets
### Route Table
### Subnet Associations 
### NAT Gateways

## Security Groups
### ALB Security Group
### SSH Security Group
### WebServer Security Group
### DataBase Security Group
### EFS Security Group

## RDS

## EFS

## Install WordPress

### Code Snippets
```
1. create the html directory and mount the efs to it
sudo su
yum update -y
mkdir -p /var/www/html
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-03c9b3354880b36a6.efs.us-east-1.amazonaws.com:/ /var/www/html
```
```
2. install apache 
sudo yum install -y httpd httpd-tools mod_ssl
sudo systemctl enable httpd 
sudo systemctl start httpd
```
```
3. install php 7.4
sudo amazon-linux-extras enable php7.4
sudo yum clean metadata
sudo yum install php php-common php-pear -y
sudo yum install php-{cgi,curl,mbstring,gd,mysqlnd,gettext,json,xml,fpm,intl,zip} -y
```
```
4. install mysql5.7
sudo rpm -Uvh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
sudo yum install mysql-community-server -y
sudo systemctl enable mysqld
sudo systemctl start mysqld
```
```
5. set permissions
sudo usermod -a -G apache ec2-user
sudo chown -R ec2-user:apache /var/www
sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;
sudo find /var/www -type f -exec sudo chmod 0664 {} \;
chown apache:apache -R /var/www/html 
```
```
6. download wordpress files
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
cp -r wordpress/* /var/www/html
```
```
7. create the wp-config.php file
cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
```
```
8. edit the wp-config.php file
nano /var/www/html/wp-config.php
```
```
9. restart the webserver
service httpd restart
```

## ALB
### Target Group

## Route 53

## Create a Certificate Manager

## HTTPS Listener For ALB

```
/* SSL Settings */
define('FORCE_SSL_ADMIN', true);

// Get true SSL status from AWS load balancer
if(isset($_SERVER['HTTP_X_FORWARDED_PROTO']) && $_SERVER['HTTP_X_FORWARDED_PROTO'] === 'https') {
  $_SERVER['HTTPS'] = '1';
```
## Encryption
```
/* SSL Settings */
define('FORCE_SSL_ADMIN', true);
// Get true SSL status from AWS load balancer
if(isset($_SERVER['HTTP_X_FORWARDED_PROTO']) && $_SERVER['HTTP_X_FORWARDED_PROTO'] === 'https') {
  $_SERVER['HTTPS'] = '1’;
```
## WordPress Theme Selection

## Final Output 
