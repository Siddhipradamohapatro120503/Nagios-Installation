# 🚀 Installing Nagios 4 on Ubuntu 20.04: A Step-by-Step Guide

By Siddhiprada Mohapatro

## 📋 Overview

Installing Nagios 4 on Ubuntu 20.04 involves several steps, including installing prerequisite software, downloading Nagios 4, configuring Nagios, and installing the Nagios plugins. This guide will walk you through the process.

## 🛠️ Installation Steps

### 1. Update the System

First, update your system to ensure you have the latest security updates and software packages:

```bash
sudo apt update && sudo apt upgrade
```

### 2. Install Necessary Packages

Install the required packages to build and install Nagios 4:

```bash
sudo apt install -y wget build-essential apache2 php openssl perl make php-gd libgd-dev libapache2-mod-php libperl-dev libssl-dev daemon autoconf libc6-dev libmcrypt-dev libssl-dev libnet-snmp-perl gettext unzip
```

### 3. Download Nagios 4

Download the latest stable release of Nagios 4:

```bash
cd /tmp
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.6.tar.gz
```

### 4. Create Nagios User and Group

Create a Nagios user and group:

```bash
sudo useradd nagios
sudo groupadd nagcmd
sudo usermod -a -G nagcmd nagios
```

### 5. Extract and Install Nagios 4

Extract the Nagios 4 archive and install it:

```bash
tar -xzf nagios-4.4.6.tar.gz
cd nagios-4.4.6
sudo ./configure --with-httpd-conf=/etc/apache2/sites-enabled
sudo make all
sudo make install
sudo make install-init
sudo make install-commandmode
sudo make install-config
sudo /usr/bin/install -c -m 644 sample-config/httpd.conf /etc/apache2/sites-enabled/nagios.conf
```

### 6. Configure Apache

Configure Apache:

```bash
sudo a2enmod rewrite
sudo a2enmod cgi
sudo systemctl restart apache2
```

### 7. Install Nagios Plugins

Install the Nagios plugins:

```bash
cd /tmp
wget https://nagios-plugins.org/download/nagios-plugins-2.3.3.tar.gz
tar -xzf nagios-plugins-2.3.3.tar.gz
cd nagios-plugins-2.3.3
sudo ./configure --with-nagios-user=nagios --with-nagios-group=nagios --with-openssl
sudo make
sudo make install
```

### 8. Verify Nagios Installation

Verify the Nagios installation:

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

You should see "Total Warnings: 0" and "Total Errors: 0" if everything is working correctly.

### 9. Start Nagios and Apache Services

Start the Nagios and Apache services:

```bash
sudo systemctl enable --now nagios.service
sudo systemctl restart apache2.service
```

## 🔐 Adding Password for Nagios Web Interface (Optional)

1. Create a Nagios user:
   ```bash
   sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
   ```

2. Configure Apache to use the htpasswd file:
   Edit `/etc/apache2/sites-enabled/nagios.conf` and replace:
   ```
   AuthUserFile /dev/null
   AuthGroupFile /dev/null
   AuthType basic
   ```
   with:
   ```
   AuthUserFile /usr/local/nagios/etc/htpasswd.users
   AuthGroupFile /dev/null
   AuthType basic
   AuthName "Restricted Access"
   ```

3. Restart Apache:
   ```bash
   sudo systemctl restart apache2.service
   ```

## 🌐 Accessing Nagios Web Interface

Access the Nagios web interface by visiting your server's IP address or hostname in a web browser, followed by "/nagios".

## ℹ️ Checking Nagios Version

To check the version of Nagios 4:

```bash
nagios4 --version
```

🎉 Congratulations! You have successfully installed Nagios 4 on Ubuntu 20.04. If you encounter any issues, please feel free to leave a comment.
