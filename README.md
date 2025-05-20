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
sudo git clone https://github.com/paknux/crudsiswa.git /var/www/html
sudo chmod -R 777 /var/www/html
echo DB_USER=admin > /var/www/html/.env
echo DB_PASS=P4ssw0rd123  >> /var/www/html/.env
echo DB_NAME=crudsiswa  >> /var/www/html/.env
echo DB_HOST=rds11tjkt1.czt6n8ylfvyb.us-east-1.rds.amazonaws.com >> /var/www/html/.env
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
Masuk ke AWS Console > EC2.
Klik Launch Instance.
Isi:
Name: TugasEC2
AMI: Ubuntu Server 22.04
Instance type: t2.micro
Key pair: Pilih vockey
Security Group:
Buka port 22 (SSH)
Buka port 80 (HTTP)
Buka port 443 (HTTPS)
Advence Details lalu scroll sampai menemukan UserData
Isi UserData sebagai berikut:
```.env
#!/bin/bash
echo '#!/bin/bash
sudo apt update -y
sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
sudo rm -rf /var/www/html/{,.}
sudo git clone https://github.com/joshuaarafael/psat2425.git /var/www/html
sudo chmod -R 777 /var/www/html
echo DB_USER=admin > /var/www/html/.env
echo DB_PASS=joshuaRAFAEL82472  >> /var/www/html/.env
echo DB_NAME=crudsiswa  >> /var/www/html/.env
echo DB_HOST=psat2425-16.culvipwxmiuk.us-east-1.rds.amazonaws.com >> /var/www/html/.env
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
#

## 2. Jalankan 
Jalankan dengan username dan password default berikut ini
#
### username = admin
### password = 123
#

Kemudian inputkanlah data sesuai dengan datamu
