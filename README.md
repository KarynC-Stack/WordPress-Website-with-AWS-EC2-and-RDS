# Creating a WordPress website using Amazon EC2 and Amazon RDS

1. Launch the RDS instance for the MYSQL database
2. Launch Amazon Linux 2 EC2 instance 
3. Install and configure WordPress 

**Connect to your EC2 instance through SSH (Use CLI or EC2 connect)**
```bash
ssh -i wordpress-key.pem ec2-user@public-ip-address
```

**Update packages on the EC2 instance** 
 ```bash
sudo yum update -y
```

**Install, start, and enable the Apache web server**
``bash
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```

**Install PHP Software**
 ```bash
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
```

**Install MySQL client**
'''bash 
sudo yum install -y mysql
'''

**Connect to RDS database using the RDS database endpoint**
``bash
mysql -h <rds-database-endpoint> -P <port-no> -u <user> -p <password>
```

**Create a database, user and give permissions to user**
```bash
CREATE DATABASE wordpress;
CREATE USER 'adminuser' IDENTIFIED BY 'Brownie25';
GRANT ALL PRIVILEGES ON wordpress.* TO adminuser;
FLUSH PRIVILEGES;
Exit
```

**Download the WordPress template and unzip the WordPress **
```bash
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
```

**Change directory to WordPress**
```bash
cd wordpress
```

**Create a wp-config file from the sample**
```bash
cp wp-config-sample.php wp-config.php
```

**Edit the wp-config file (Use vim or nano)**
```bash
vim wp-config.php
```

**Edit the database name, username, password, and hostname. Replace with details used to launch your RDS instance.**
``bash
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'database_name_here' );

/** MySQL database username */
define( 'DB_USER', 'username_here' );

/** MySQL database password */
define( 'DB_PASSWORD', 'password_here' );

/** MySQL hostname */
define( 'DB_HOST', 'rds-endpoint-name' );
```

**Go to the below link to get information on updating the wp-config file**
```bash
[https://api.wordpress.org/secret-key/1.1/salt/]
```

**Configure the Authentication Unique Keys and Salts and update the file**
```bash
define('AUTH_KEY',         '+7CA?k*Ju&8eCfg=/aFKo0tO5Tn73Cg 9|Ed73k|Gw(3^');
define('SECURE_AUTH_KEY',  ':H$M&FvbE6t:EwH5ik/D!@]@%Dv3!-Q^hNH3*O+-$L6c*|');
define('LOGGED_IN_KEY',    'g9?;b_A BNW[; $9N^E2^jt$LkF 8_^baTmjhp<eE5GUd');
define('NONCE_KEY',        'G;Wf@|;jzQh>R812&-x^cPoq`tOOu>q)#JVa Y%No%.JpZ[');
define('AUTH_SALT',        'up^dE)4&x/?]1[thjghhjjhz6Vhiohr(dVMh+d5=R<.l_#l');
define('SECURE_AUTH_SALT', '@%ka=9?}BQ[m#29D+@jkgjkhjkhjkhkjhkjdTZ`MT{|fypE~');
define('LOGGED_IN_SALT',   'o!UX5|LW4eijhjkbhkjhkjkjbnjjb/1JSPS?e`YW*nrWb|FG ');
define('NONCE_SALT',       '+t}kH4DA`jhbjkbjkbjkbjkbjkbt8(iWX(]e?&tV;k:>|)IoE');
```

**Install application dependencies for WordPress**
```bash
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
```

**Go back to the home directory and copy the content to path /var/www/html**
```bash
cd /home/ec2-user        
sudo cp -r wordpress/* /var/www/html/
```

**Restart the Apache service**
```bash
sudo service httpd restart
```

**Paste your public ip address on your web browse to go to the wordpress configuration page
