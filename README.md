# Penilaian Sumatif Akhir Tahun
## Mapil DevOps XI TJKT 1 - Penilaian Praktek
### SMKN 1 Banyumas - TA. 2024 2025


#
# Cara mendeploy Aplikasi

## 1. Buat File .env

File .env adalah file environment sistem mirip seperti file konfig.php
#
isi file .env sebagai berikut

```.env
DB_USER=....  (isi dengan user RDS)
DB_PASS=....  (isi dengan password RDS)
DB_NAME=....  (isi dengan nama database yang akan dibuat di RDS)
DB_HOST=....  (isi dengan Endpoint RDS)
```

contoh:

```.env
DB_USER=admin
DB_PASS=P4ssw0rd123
DB_NAME=psat2425
DB_HOST=rdsku.czt6n8ylfvyb.us-east-1.rds.amazonaws.com
```

#
# Perintah untuk deploy
```.env
sudo apt update -y

sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client

sudo rm -rf /var/www/html/{*,.*}

sudo git clone https://github.com/[github mu] /var/www/html

sudo chmod -R 777 /var/www/html

echo DB_USER=[username rds] > /var/www/html/.env
echo DB_PASS=[password rds]  >> /var/www/html/.env
echo DB_NAME=crudsiswa  >> /var/www/html/.env
echo DB_HOST=[endpoint rds] >> /var/www/html/.env

sudo apt install openssl
sudo a2enmod ssl
sudo a2ensite default-ssl.conf
sudo systemctl reload apache2
```
atau bisa dibuat menjadi shell script, misal diberi nama otomatis.sh

### otomatis.sh
```.env
#!/bin/bash
sudo apt update -y
sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
sudo rm -rf /var/www/html/{*,.*}
sudo git clone https://github.com/[github mu] /var/www/html
sudo chmod -R 777 /var/www/html
echo DB_USER=[username rds] > /var/www/html/.env
echo DB_PASS=[password rds]  >> /var/www/html/.env
echo DB_NAME=crudsiswa  >> /var/www/html/.env
echo DB_HOST=[endpoint rds] >> /var/www/html/.env
sudo apt install openssl
sudo a2enmod ssl
sudo a2ensite default-ssl.conf
sudo systemctl reload apache2
```

### cara menjalankan otomatis.sh setidaknya ada 2 cara
1. Dieksekusi langsung
```.env
$ nano otomatis.sh (lalu isikan file didalamnya sama seperti yang ada pada otomatis.sh)
$ chmod +x otomatis.sh
$ ./otomatis.sh
```

2. menggunakan UserData pada EC2
```.env
- Masuk ke AWS Console > EC2.
- Klik Launch Instance.
- Isi:
- Name: TugasEC2
- AMI: Ubuntu Server 22.04
- Instance type: t2.micro
- Key pair: Pilih vockey
- Security Group:
Buka port 22 (SSH)
Buka port 80 (HTTP)
Buka port 443 (HTTPS)
- Advence Details lalu scroll sampai menemukan UserData
- Isi UserData sebagai berikut:
```
```.env
#!/bin/bash
echo '#!/bin/bash
sudo apt update -y
sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
sudo rm -rf /var/www/html/{,.}
sudo git clone https://github.com/[github mu] /var/www/html
sudo chmod -R 777 /var/www/html
echo DB_USER=[username rds] > /var/www/html/.env
echo DB_PASS=[password rds]  >> /var/www/html/.env
echo DB_NAME=crudsiswa  >> /var/www/html/.env
echo DB_HOST=[endpoint rds] >> /var/www/html/.env
sudo apt install -y openssl
sudo a2enmod ssl
sudo a2ensite default-ssl.conf
sudo systemctl reload apache2' > /home/ubuntu/otomatis.sh

chmod +x /home/ubuntu/otomatis.sh  
```

- Eksekusi
```.env
$ ./otomatis.sh
```
### memang terlihat sama saja namun cara yang ke-2 lebih menghemat waktu karena kita tinggal menambahkan UserData saat menginstal EC2 lalu tinggal mengetikkan " ./ otomatis.sh " maka kode yang ada didalam UserData akan langsung serentak dijalankan.

#

## 2. Jalankan 
Jalankan dengan username dan password default berikut ini
#
### username = admin
### password = 123
#

Kemudian inputkanlah data sesuai dengan datamu

# Catatan tambahan
1. Cara membuat Security Group
```.env
- Cari VPC pada kolom pencarian
- Cari Security Groups
- Create Security Group
- Lalu ikuti langkah dibawah ini:
```
```.env
1. Buat security group SG-ServerDB
   allow inbound  MySQL (3306) from anywhereIPv4 (0.0.0.0/0)  

2. Buat security group SG-ServerWeb
   allow inbound SSH (22) from anywhereIPv4 (0.0.0.0/0)  
   allow inbound HTTP (80) from anywhereIPv4 (0.0.0.0/0)  
   allow inbound HTTPS (443) from anywhereIPv4 (0.0.0.0/0)  
```

2. Cara membuat RDS
```.env
- Cari Aurora and RDS
- Create Database
   Engine Type : pilih MySQL
   Templates : Free tier
   DB cluster identifier : (nama bebas misal : serverdb)
   Master Username : (biarkan admin)
   Master Password : isi misal P4ssw0rd
   Public access : No 
   Untuk Security group pastikan buat/pilih yang accept port inbound 3306 dari 0.0.0.0/0 (SG-ServerDB)
- Klik Create Database
- Tunggu sampai EndPoint muncul, bisa dicek dengan klik RDS yang kita buat lalu cari endpoint & port
```