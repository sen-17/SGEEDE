# **PREPARING ENVIRONMENT**
Installing and setting up Odoo in Linux

## **Update the Server (Ubuntu 22.04+)**
```sudo apt update && sudo apt upgrade -y```
```sudo apt install libpq-dev```

## **Install PostgreSQL 17**
```# Add PostgreSQL repository
sudo curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list
sudo apt update
sudo apt install postgresql-17 -y
sudo su - postgres -c "createuser -s sgeede" 2> /dev/null || true
```

## **Install Python and System Dependencies**
```
sudo apt install python3 python3-pip
sudo apt install git build-essential wget python3-dev python3-venv python3-wheel \
libxslt-dev libzip-dev libldap2-dev libsasl2-dev python3-setuptools node-less \
libpng-dev libjpeg-dev gdebi -y
```

## **Install Python Packages for Odoo 18**
```sudo -H pip3 install -r https://github.com/odoo/odoo/raw/18.0/requirements.txt```

## **Install NodeJS and rtlcss**
```sudo apt install nodejs npm -y```
```sudo npm install -g rtlcss less less-plugin-clean-css```

## **Install Wkhtmltopdf (for PDF reports)**
```sudo apt install wkhtmltopdf -y```

## **Create Odoo System User and Log Path**
```sudo adduser --system --quiet --shell=/bin/bash --home=/sgeede --gecos 'ODOO' --group sgeede
sudo adduser sgeede sudo
sudo mkdir /var/log/sgeede
sudo chown sgeede:sgeede /var/log/sgeede
```

## **Clone Odoo 18 Source Code**
```
sudo git clone --depth 1 --branch 18.0 https://github.com/odoo/odoo /opt/sgeede-server
```

## **Set File Permissions and Create Config**
```
sudo chown -R sgeede:sgeede /sgeede/*
sudo touch /etc/sgeede-server.conf
sudo su root -c "printf '[options]\n' >> /etc/sgeede-server.conf"
sudo su root -c "printf 'admin_passwd = $(cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 16 | head -n 1)\n' >> /etc/sgeede-server.conf"
sudo su root -c "printf 'xmlrpc_port = 8069\n' >> /etc/sgeede-server.conf"
sudo su root -c "printf 'logfile = /var/log/sgeede/sgeede-server.log\n' >> /etc/sgeede-server.conf"
sudo su root -c "printf 'addons_path=/opt/sgeede-server/addons\n' >> /etc/sgeede-server.conf"

sudo chown sgeede:sgeede /etc/sgeede-server.conf
sudo chmod 640 /etc/sgeede-server.conf
```

## **Create Startup Script**
```sudo su root -c "echo '#!/bin/sh' >> /opt/sgeede-server/start.sh"
sudo su root -c "echo 'sudo -u sgeede /opt/sgeede-server/odoo-bin --config=/etc/sgeede-server.conf' >> /opt/sgeede-server/start.sh"
sudo chmod 755 /opt/sgeede-server/start.sh
```

## **(Optional) Install and Configure Nginx**
```
sudo apt install nginx -y

cat <<EOF > ~/sgeede
upstream sgeede {
  server 127.0.0.1:8069 weight=1 fail_timeout=0;
}
upstream sgeede-im {
  server 127.0.0.1:8072 weight=1 fail_timeout=0;
}
map \$http_upgrade \$connection_upgrade {
  default upgrade;
  '' close;
}
server {
  server_name sgeede;
  access_log /var/log/nginx/sgeede-access.log;
  error_log /var/log/nginx/sgeede-error.log;

  location /websocket {
    proxy_pass http://sgeede-im;
    proxy_set_header Upgrade \$http_upgrade;
    proxy_set_header Connection \$connection_upgrade;
    proxy_set_header X-Forwarded-Host \$host;
    proxy_set_header X-Real-IP \$remote_addr;
  }

  location / {
    proxy_pass http://sgeede;
    proxy_set_header Host \$host;
    proxy_set_header X-Real-IP \$remote_addr;
  }

  gzip on;
  gzip_min_length 1000;
  client_max_body_size 100M;
}
EOF

sudo mv ~/sgeede /etc/nginx/sites-available/sgeede
sudo ln -s /etc/nginx/sites-available/sgeede /etc/nginx/sites-enabled/sgeede
sudo rm /etc/nginx/sites-enabled/default
sudo service nginx reload

sudo su root -c "printf 'proxy_mode = True\n' >> /etc/sgeede-server.conf"
```

## **Run Odoo**
- To start:
```
/etc/init.d/sgeede-server start
```

- Or run manually in local dev:
```
/opt/sgeede-server/odoo-bin --config=/etc/sgeede-server.conf
```

