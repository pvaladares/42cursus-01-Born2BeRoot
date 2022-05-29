# Table of Contents

- [1 - Virtual Machine (VirtualBox)](#1---virtual-machine-virtualbox)
- [2 - Debian Installation](#2---debian-installation)
	- [2.1 - General](#21---general)
	- [2.2 - Disk partition (with bonus)](#22---disk-partition-with-bonus)
	- [2.3 - Final installation steps](#23---final-installation-steps)
	- [2.4 - Reboot, unlock the disk, login and check disk partitions](#24---reboot-unlock-the-disk-login-and-check-disk-partitions)
- [3 - Mandatory part](#3---mandatory-part)
	- [3.1 - apt and sudo](#31---apt-and-sudo)
	- [3.2 - Adding in groups](#32---adding-in-groups)
	- [3.3 - Setting up a strong password policy](#33---setting-up-a-strong-password-policy)
		- [3.3.1 - login.defs - shadow password suite configuration](#331---logindefs---shadow-password-suite-configuration)
		- [3.3.2 - pam_pwquality - PAM module to perform password quality checking](#332---pam_pwquality---pam-module-to-perform-password-quality-checking)
		- [3.3.3 - sudoers - default sudo security policy plugin](#333---sudoers---default-sudo-security-policy-plugin)
	- [3.4 ssh](#34-ssh)
		- [3.4.1 - Installation](#341---installation)
		- [3.4.2 - Configuration](#342---configuration)
		- [3.4.3 - Connection from terminal](#343---connection-from-terminal)
	- [3.5 ufw](#35-ufw)
	- [3.6 cron](#36-cron)
- [4 Bonus part](#4-bonus-part)
	- [4.1 WordPress website](#41-wordpress-website)
		- [4.1.1 lighttpd](#411-lighttpd)
		- [4.1.2 MariaDB](#412-mariadb)
		- [4.1.3 php](#413-php)
		- [4.1.4 WordPress](#414-wordpress)
	- [4.2 IPFS](#42-ipfs)
	- [4.3 Final checks](#43-final-checks)
- [5 - Preparation for Defense](#5---preparation-for-defense)
	- [5.1 - Operating System (Debian), Package Managers (aptitude vs apt) and AppArmor](#51---operating-system-debian-package-managers-aptitude-vs-apt-and-apparmor)
	- [5.2 - ssh](#52---ssh)
	- [5.3 - ufw](#53---ufw)
	- [5.4 - Users and Groups](#54---users-and-groups)
	- [5.5 - cron](#55---cron)
- [6 - Resources](#6---resources)



# 1 - Virtual Machine (VirtualBox)

1 - On the 42 machine, launch the [Managed Software Center](img/VM/1.png) app and search/install [VirtualBox 5.2.20](img/VM/2.png) which is an old version since at the time of writing the latest version is 6.1 as can be checked [here](https://www.virtualbox.org/). To be noted that the alternative software `UTM` stated on the subject cannot be used since there are no admin privileges to install it nor it is available on the `Managed Software Center` to download/install. So `VirtualBox` is indeed the mandatory software to be used on this project.

2 - [Download](https://www.debian.org/download) the latest stable version of Debian and place it on your `sgoinfre` user folder (since it is the least restrictive on size limit).

3 - Step by step instructions to create VM
* Launch `VirtualBox`
* [Select](img/VM/3.png) `New` and type `Born2beRoot` as name, choose `Linux` type and `Debian (64-bit)` version.
* Leave [memory size](img/VM/4.png) as default `1024 MB`

4 - If doing the **bonus**, since there is a limitation with the 5.2.20 version use the workaround described below:

*Note: The 5.2.20 version - the only one that can be used at 42 - has a [limitation/bug](https://www.virtualbox.org/ticket/18177) with impact on the bonus, since it will not be possible to set a disk with size exactly to 30.8G*
* Option 1 - Attempt to create the size exactly 30.8G as per the subject
  * Go to terminal, and still at `sgoinfre` user folder, use the [`VBoxManage`](https://www.virtualbox.org/manual/ch08.html) command, type `VBoxManage createhd --filename Born2beRoot_DISK_31G54.vdi --size 31540 --format VDI`
  * Choose `Use an existing virtual hard disk file` and [select the newly created file](img/VM/5.png)
  * [Go to](img/VM/6.png) `Settings` and `Storage`, under `Storage Devices` choose `Empty` and then click on the "CD/Disk" icon on the right of the `Optical Drive` and select option `Choose Virtual Optical Disk File...`; [Select](img/VM/7.png) the debian ISO file that was privously saved on goinfre
* Option 2 - Just set a size without decimals like 30G (afterall the important thing is to follow the structure from the subject)
 
5 - Now just `Start` the VM to start the installation procedure of Debian.

# 2 - Debian Installation

## 2.1 - General
* [Select](img/Install/1.png) `Install` instead of any other option (remember, no graphical install is allowed)
* [Language](img/Install/2.png): select default `English`
* [Location](img/Install/3.png): select `other`, `Europe` and then `Portugal`
* [Local configuration](img/Install/4.png): select default `United States`
* [Keyboard](img/Install/5.png): `American English`
> - The hostname of your virtual machine must be your login ending with 42 (e.g., wil42)
* [Hostname](img/Install/6.png): `pvaladar42`
* > Note: Hostname can always be changed later using command `hostnamectl set-hostname <name>`, more info [here](https://www.cyberciti.biz/faq/debian-change-hostname-permanently/)
* [Domain name](img/Install/7.png): *empty*
> - You have to implement a strong password policy.
> 
> To set up a strong password policy, you have to comply with the following requirements:
> - (...)
> - Your password must be at least 10 characters long. It must contain an uppercase letter and a number. Also, it must not contain more than 3 consecutive identical characters.
> - The password must not include the name of the user.
* Root password: *see rules above (e.g.: dcOivR999D, use a strong password generator like [here](https://passwordsgenerator.net/))*
> In addition to the root user, a user with your login as username has to be present.
* User account:
  * [Full name](img/Install/8.png): `Pedro Valadares`
  * [Username](img/Install/9.png): `pvaladar`
 * Password: *see rules above (e.g.: zipNNxuk1p)*
* [Timezone](img/Install/10.png): `Lisbon`

## 2.2 - Disk partition (with bonus)

* For the disk there is a really nice tutorial [here](https://youtu.be/OQEdjt38ZJA?t=185).

## 2.3 - Final installation steps

* Configure the package manager:
  * [Scan extra installation media](img/Install/11.png): choose `No`
  * [Debian archive mirror country](img/Install/12.png): choose `Portugal`
  * [Debian archive mirror](img/Install/13.png): choose `deb.debian.org`
  * [HTTP proxy information](img/Install/14.png): *empty*
* [Participate in the package usage survey](img/Install/15.png): choose `No`
* [Software selection](img/Install/16.png): *deselect all*
* Install the GRUB boot loader:
  * [Install the GRUB boot loader to your primary drive](img/Install/17.png): choose `Yes`
  * [Device for boot loader installation](img/Install/18.png): choose `/dev/sda`
* [Finish the installation](img/Install/19.png): choose `Continue`

## 2.4 - Reboot, unlock the disk, login and check disk partitions

* Now the machine will reboot and the [GRUB boot loader will show Debian](img/Install/20.png) and load it by default, if nothing is pressed
* Type the [unlock disk password](img/Install/21.png), previously set during the installation, to unlock the disk
* Then type the [username `pvaladar` and the associated password](img/Install/22.png)
* In order to cross-check the disk partition *versus* the subject (in this case the bonus), use the [`lsblk` utility](img/Install/23.png)

# 3 - Mandatory part

## 3.1 - `apt` and `sudo`
* First procedure would be to login as root [`su --login`](https://man7.org/linux/man-pages/man1/su.1.html) and upgdate & upgrade the default package manager APT *(Advanced Packaging Tool)* `apt update && apt upgrade`. For more information install man `apt install man` and read the docs `man apt`
> You have to install and configure sudo following strict rules
* The second step is to install sudo since there are several requirements with permissions of users/groups, etc. that need to be modified/set: `apt install sudo`

## 3.2 - Adding in groups

> - In addition to the root user, a user with your login as username has to be present.
>
> - This user has to belong to the `user42` and `sudo` groups.

* Add the user to sudo `adduser pvaladar sudo`
* Reboot with `reboot` and then login again after `sudo -v` to force cached credentials (will confirm that sudo is working)
* Create the group `sudo addgroup user42`
* Add the user to the group `sudo adduser pvaladar user42`
* Verify if the user is correctly added to the groups: `getent group sudo` and `getent group user42`

*Note: Another way would be to use the following command: `usermod -aG sudo pvaladar`*, man page [here](https://man7.org/linux/man-pages/man8/usermod.8.html)

## 3.3 - Setting up a strong password policy

### 3.3.1 - [login.defs - shadow password suite configuration](https://manpages.debian.org/jessie/login/login.defs.5.en.html)

Use editor to change the target file: `/etc/login.defs`

> To set up a strong password policy, you have to comply with the following requirements:
>
> - Your password has to expire every 30 days.
* On line 160, replace `PASS_MAX_DAYS    99999` by `PASS_MAX_DAYS    30`
> - The minimum number of days allowed before the modification of a password will be set to 2.
* On line 161, replace `PASS_MIN_DAYS    0` by `PASS_MIN_DAYS    2`
> - The user has to receive a warning message 7 days before their password expires.
* On line 162, there is no need to change since `PASS_WARN_AGE` is already 7 by default.


Notice that on line 296 it is mentioned that another file needs to be edited on folder `/etc/pam.d/`
```bash
################# OBSOLETED BY PAM ##############
#                                               #
# These options are now handled by PAM. Please  #
# edit the appropriate file in /etc/pam.d/ to   #
# enable the equivelants of them.
#
###############
```

For existing users like `root` and `pvaladar` it is required to manually update them:
```bash
# Update root
chage root --maxdays 30 --mindays 2 --warndays 7
# Confirm that everything is OK with root
chage -l root
# Update user
chage pvaladar -M 30 -m 2 -W 7
# Confirm that everything is OK with user
chage -l pvaladar
```

### 3.3.2 - [pam_pwquality - PAM module to perform password quality checking](https://manpages.debian.org/jessie/libpam-pwquality/pam_pwquality.8.en.html)

In order to enforce the remaining rules there is the package `pwquality` that can handle it for us:

```bash
apt install libpam-pwquality
```

Use editor to change the target file: `/etc/pam.d/common-password` and add the following to the right of `pam_pwquality.so retry=3`:

```bash
minlen=10 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root
```

Below it is explained each argument as per the subject requirements.

> - Your password must be at least 10 characters long. 
* `minlen=10`
> - It must contain an uppercase letter and a number. 
* `ucredit=-1` and `dcredit=-1`
> - Also, it must not contain more than 3 consecutive identical characters. 
* `maxrepeat=3`
> - The password must not include the name of the user.
* `reject_username`
> - The following rule does not apply to the root password: The password must have at least 7 characters that are not part of the former password.
* `difok=7`
> - Of course, your root password has to comply with this policy.
* `enforce_for_root`


### 3.3.3 - [sudoers - default sudo security policy plugin](https://man7.org/linux/man-pages/man5/sudoers.5.html)

Edit the sudoers `/etc/sudoers` file in order to comply with requirements, by using the command `sudo visudo`

See that they recommend the following:
```bash
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file
```

So create the following file `/etc/sudoers.d/sudoconfig` and add the content:
```bash
Defaults    passwd_tries=3
Defaults    badpass_message="42 Message: Incorrect sudo password, you have a total of 3 tries."
Defaults    log_input,log_output
Defaults    iolog_dir="/var/log/sudo"
Defaults    requiretty
Defaults    secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```

> To set up a strong configuration for your sudo group, you have to comply with the following requirements:
> - Authentication using sudo has to be limited to 3 attempts in the event of an incorrect password.
* `Defaults    passwd_tries=3`
> - A custom message of your choice has to be displayed if an error due to a wrong password occurs when using sudo.
* `Defaults    badpass_message="42 Message: Incorrect sudo password, you have a total of 3 tries."`
> - Each action using sudo has to be archived, both inputs and outputs. The log file has to be saved in the /var/log/sudo/ folder.
* `Defaults    log_input,log_output`
> - The TTY mode has to be enabled for security reasons.
* `Defaults    requiretty`
> - For security reasons too, the paths that can be used by sudo must be restricted. 
> > Example:
> /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin
* `Defaults    secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"`

Just need to create the folder to store the logs

```bash
mkdir /var/log/sudo
```

> After setting up your configuration files, you will have to change all the passwords of the accounts present on the virtual machine,  including the root account.

Make final arrangements by changing the previously passwords set for username and root accordingy to the new policy set (if you followed my instructions, the password should be already aligned with the strong password policy):

```bash
passwd # to change user password
sudo passwd # to change root password
```

## 3.4 `ssh`

### 3.4.1 - Installation
* Access super user privileges by typing `su --login` and entering the previously set password
* Install SSH by typing `apt install openssh-server` (this installs both server and client)
* Make the following checks around openssh-server:
  * It is installed: `dpkg -l | grep ssh`
  * It is active (running): `systemctl status ssh`

### 3.4.2 - Configuration
 
* Edit the configuration file `/etc/ssh/sshd_config`
> A SSH service will be running on port 4242 only.
* On line 15, replace `#Port 22` by `Port 4242`
* Double-check the port is correctly configured by using command `cat /etc/ssh/sshd_config | grep Port`
> For security reasons, it must not be possible to connect using SSH as root.
* On line 34, replace `#PermitRootLogin prohibit-password` by `PermitRootLogin no`
* Double-check the root permission is correctly configured by using command `cat /etc/ssh/sshd_config | grep PermitRootLogin`
* Now need to restart the SSH service to replace the new port, type `systemctl restart ssh` and then `systemctl status ssh`, [it should be read something like *Server listening on 0.0.0. port 4242*](img/VM/8.png)

### 3.4.3 - Connection from terminal

* On server machine type `ip addr` to know the <ip address>.
* Under `VirtualBox/Network` choose [`Bridged Adapter`](img/VM/9.png)
* On terminal type `ssh pvaladar@<ip address> -p 4242` and enter the associated password. When ready type `exit` or `logout` to end the connection
 
## 3.5 `ufw`

> You have to configure your operating system with the UFW firewall and thus leave only port 4242 open.
> 
> Your firewall must be active when you launch your virtual machine.
* Use command `apt install ufw` to install the firewall package and `ufw status` to check status (it should say "inactive").
* Type `ufw enable` so the *Firewall is active and enabled on system startup*
* Type `ufw allow 4242` and then `ufw status numbered`, only the port 4242 should appear on the list
* `ufw status` and `systemctl status ufw` should show now it as "active"

## 3.6 `cron`

> At server startup, the script will display some information (listed below) on all terminals every 10 minutes (take a look at wall). 
> The banner is optional.
* Broadcast banner can be hidden using the switch `wall --nobanner`
> No error must be visible.
* Use `sudo crontab -u root -e` to edit the scheduled commands and add the line `*/10 * * * * bash /usr/local/sbin/monitoring.sh` (more info about `crontab` [here](https://crontab.guru/#*/10__**))

*Note: for time less than 1 minute, let's say 30s, it would be required a [workaround using `sleep`](https://stackoverflow.com/questions/9619362/running-a-cron-every-30-seconds):*
```bash
* * * * * bash /usr/local/sbin/monitoring.sh
* * * * * sleep 30s ; bash /usr/local/sbin/monitoring.sh
```
> Finally, you have to create a simple script called monitoring.sh. It must be developed in bash.

* Create the script below and use `sudo chmod +x /usr/local/sbin/monitoring.sh`

```bash
#!/bin/bash
#
# • The architecture of your operating system and its kernel version.
# Altough only operating system and kernel version are mentioned, the screenshot of the wall output shows all the information printed by uname
#   uname - print system information
architecture=$(uname --all)
#
# • The number of physical processors.
# Count the number of unique core ids to get the number of physical cores on a machine
# https://www.baeldung.com/linux/get-number-of-processors
#   grep - print lines that match patterns
#   sort - sort lines of text files
#   wc - print newline, word, and byte counts for each file
physical_cpu=$(grep "^physical id" /proc/cpuinfo | sort --unique | wc --lines)
#
# • The number of virtual processors.
# Count the exact number of virtual cpu (vCPU)
# https://webhostinggeeks.com/howto/how-to-display-the-number-of-processors-vcpu-on-linux-vps/
virtual_cpu=$(grep --count "^processor" /proc/cpuinfo)
#
# • The current available RAM on your server and its utilization rate as a percentage.
# Get information about memory in MB units (refer to the wall output) for Memory (RAM) only
#   free - Display amount of free and used memory in the system
#   awk - pattern scanning and text processing language
#   printf - format and print data
memory_usage=$(free --mega | awk '$1 == "Mem:" {printf "%s/%sMB (%.2f%%)\n", $3, $2, $3*100/$2}')
#
# • The current available memory on your server and its utilization rate as a percentage.
# Compute total disk in GiB (G) units and total used disk in MiB (M) units (refer to the wall output)
#   df - report file system disk space usage
disk_usage=$(df --block-size=M --total | awk '$1 == "total" {printf "%d/%dGb (%s)\n", $3, $2/1000, $5}')
#
# • The current utilization rate of your processors as a percentage.
# https://www.tecmint.com/understand-linux-load-averages-and-monitor-performance/
#   top - display Linux processes
cpu_load=$(top -bn1 | grep load | awk '{printf "%.2f%%\n", $(NF-2)}')
#
# • The date and time of the last reboot.
#   who - show who is logged on
last_boot=$(who --boot | awk '$1 == "system" {print $3 " " $4}')
#
# • Whether LVM is active or not.
# Checks if the type of devices includes at least one "lvm" type
# https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php
#   lsblk - list block devices
lvm_partitions=$(lsblk | awk '$(NF-1) == "lvm"' | wc --lines)
lvm_is_used=$(if [ $lvm_partitions -eq 0 ]; then echo no; else echo yes; fi)

# • The number of active connections.
# Check the content of files sockstat...sockstat6
# [$ sudo apt install net-tools]
tcp_connections=$(cat /proc/net/sockstat{,6} | awk '$1 == "TCP:" {print $3}')
#
# • The number of users using the server.
# Get the number of user by removing the header and count each line equal one user
#   w - Show who is logged on and what they are doing
users_logged_in=$(w --no-header | wc --lines)
#
# • The IPv4 address of your server and its MAC (Media Access Control) address.
#   hostname - show or set the system's host name
#   ip - show / manipulate routing, network devices, interfaces and tunnels
ipv4_address=$(hostname --all-ip-addresses)
mac_address=$(ip link show | awk '$1 == "link/ether" {print $2}')
#
# • The number of commands executed with the sudo program.
#   journalctl - Query the systemd journal
sudo_commands_count=$(journalctl _COMM=sudo | grep --count COMMAND)
#
#   wall - write a message to all users
wall "
	#Architecture: $architecture
	#CPU physical: $physical_cpu
	#vCPU: $virtual_cpu
	#Memory Usage: $memory_usage
	#Disk Usage: $disk_usage
	#CPU load: $cpu_load
	#Last boot: $last_boot
	#LVM use: $lvm_is_used
	#Connexions TCP: $tcp_connections ESTABLISHED
	#User log: $users_logged_in
	#Network: IP $ipv4_address($mac_address)
	#Sudo: $sudo_commands_count cmd
"
```

> Note: In case the output is totally messed up, make sure the file format is set to unix in the editor. For example, in `VI` or `VIM` use command `:set fileformat=unix`

* Check that the scheduled job exists `sudo crontab -u root -l`
* To stop the cron service if annoying, just use `sudo service cron stop` or `sudo systemctl stop cron`

# 4 Bonus part

## 4.1 WordPress website

> Set up a functional WordPress website with the following services: lighttpd, MariaDB, and PHP.

The subject requests WordPress with some services, the [official page states the requirements](https://wordpress.org/support/article/before-you-install/
): 
> Requirements on the server side
> * **PHP** 7.4 or greater
> * **MySQL** 5.6 or **MariaDB** 10.1 or greater

The [detailed instructions](https://wordpress.org/support/article/how-to-install-wordpress/#detailed-instructions) are also found on the official website.

For the WordPress we will be installing the so called **LLMP Stack (Linux Lighttpd MariaDB PHP)**.

### 4.1.1 `lighttpd`

*lighttpd (pronounced /lighty/) is a secure, fast, compliant, and very flexible web server that has been optimized for high-performance environments. lighttpd uses memory and CPU efficiently and has lower resource use than other popular web servers. Its advanced feature-set (FastCGI, CGI, Auth, Output-Compression, URL-Rewriting and much more) make lighttpd the perfect web server for all systems, small and large.*

* Install `lighttpd` by typing the following command `sudo apt install lighttpd`
* Check it was installed and is active with both commands `dpkg -l | grep lighttpd` and `sudo service lighttpd status`
* Lastetly we need to whitelist incoming port 80 (default port for webservers) with `sudo ufw allow 80/tcp` 
* Go to host browser and navigate to `http://<ip address>`. At this point you should see the default welcome message instead of a not found webpage or ERR_CONNECTION_REFUSED message.

### 4.1.2 `MariaDB`

*MariaDB Server is one of the most popular open source relational databases. It’s made by the original developers of MySQL and guaranteed to stay open source. It is part of most cloud offerings and the default in most Linux distributions.*

* Install the package by typing the following command `sudo apt install mariadb-server`
* Check it was installed and is active with both commands `dpkg -l | grep mariadb` and `sudo service mariadb status`
* Start interactive script to improve default security settings with commmand `sudo mysql_secure_installation`
	* Enter current password for root (enter for none): *`Enter for none`*
	* Switch to unix_socket authentication [Y/n]: `n`
	* Change the root password? [Y/n]: `n`
	* Remove anonymous users? [Y/n]: `Y`
	* Disallow root login remotely? [Y/n]: `Y`
	* Remove test database and access to it? [Y/n]: `Y`
	* Reload privilege tables now? [Y/n]: `Y`
* Now let's create the database, type `sudo mariadb` to enter the mysql commands console:
```bash
CREATE DATABASE my_db;
```
* Create `pvaladar` database user identified by `IamTHEp4ssword!` password:
```bash
CREATE USER pvaladar@localhost IDENTIFIED BY 'IamTHEp4ssword!';
```
* Give full privileges on `my_db` to `pvaladar`:
```bash
GRANT ALL ON my_db.* TO 'pvaladar'@'localhost' IDENTIFIED BY 'IamTHEp4ssword!' WITH GRANT OPTION;
```
* Changes to take effect without reload/restart MariaDB:
```bash
FLUSH PRIVILEGES;
```
* Check all users and host name where they are allowed to login:
```bash
SELECT host, user FROM mysql.user;
```
* Everything should be OK now. Type `exit` and log in back to check to which databases the newly created user has access to:
```bash
sudo mariadb --user=pvaladar --password=IamTHEp4ssword!
```
* Check that the tables that were created, `my_db` should be listed:
```bash
SHOW DATABASES;
```
* Type `exit` to return to the shell

### 4.1.3 `php`

* Install the following packages:
```bash
sudo apt install php-cgi php-mysql
```

* `php-cgi`

*server-side, HTML-embedded scripting language (CGI binary)
This package provides the /usr/lib/cgi-bin/php CGI interpreter built for use in Apache 2 with mod_actions, or any other CGI httpd that supports a similar mechanism. Note that MOST users probably want the php7.3-fpm package.*

*The following extensions are built in: Core date filter hash libxml openssl pcntl pcre Reflection session sodium SPL standard zlib.*

*PHP (recursive acronym for PHP: Hypertext Preprocessor) is a widely-used open source general-purpose scripting language that is especially suited for web development and can be embedded into HTML.*

* `php-mysql`

*This package provides a MySQL module for PHP.*

*PHP (recursive acronym for PHP: Hypertext Preprocessor) is a widely-used open source general-purpose scripting language that is especially suited for web development and can be embedded into HTML.*

* Check it was installed with command `dpkg -l | grep php`

* The following commands are required to be performed, else 403 Forbidden will be shown when trying to access `<ip address>/index.php` from browser, as per [this discussion](https://stackoverflow.com/questions/11537888/lighttpd-403-forbidden-for-php-files):
```bash
# Enable FastCGI Lighttpd modules
sudo lighty-enable-mod fastcgi
sudo lighty-enable-mod fastcgi-php
# Restart server
sudo service lighttpd force-reload
```   
    
### 4.1.4 `WordPress`

* Download the latest version using `wget` (if not already, install it by command `sudo apt install wget`):
```bash
wget https://wordpress.org/latest.tar.gz
```
* Extract the content of compressed file:
```bash
tar --extract --gzip --verbose --file=latest.tar.gz
```
* Copy the extracted files to the relevant path to publish to external view:
```bash
sudo cp --recursive wordpress/* /var/www/html
```
* Delete the installation packages that are not necessary anymore:
```bash
rm -rf latest.tar.gz wordpress/
```
* Create configuration file from its sample:
```bash
sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
```
* Edit `wp-config.php` (The base configuration for WordPress) to adjust the database details to match the previously work done with the `MariaDB` configuration.
```bash
sudo vi /var/www/html/wp-config.php
```
* On line 23, replace `database_name_here` by `my_db`
* On line 26, replace `username_here` by `pvaladar`
* On line 29, replace `password_here` by `IamTHEp4ssword!`

* Now accessing `http://<ip address>` will automatically forward to `http://<ip address>/wp-admin/install.php` to start the installation.

## 4.2 IPFS

> * Set up a service of your choice that you think is useful (NGINX / Apache2 excluded!).

Since the mandatory bonus is to install a webserver, it would be interesting to install as last bonus a kind of peer-to-peer file storage service.

* [`IPFS -- Inter-Planetary File system`](https://github.com/ipfs/go-ipfs/blob/master/assets/init-doc/about)

*IPFS is a global, versioned, peer-to-peer filesystem. It combines good ideas from Git, BitTorrent, Kademlia, SFS, and the Web. It is like a single bit-torrent swarm, exchanging git objects. IPFS provides an interface as simple
as the HTTP web, but with permanence built-in.*

There are several options for installation but for simplicity and since no graphic interface is allowed the command-line version will be used.

* `Command-line install`

*All IPFS, no frills*
*Just want to use IPFS from your terminal? Follow these step-by-step instructions for getting up and running on the command line using the Go implementation of IPFS. Includes directions for Windows, macOS, and Linux.*

* Follow the [official instructions for Linux](https://docs.ipfs.io/install/command-line/#linux)
```bash
# 1 - Download the Linux binary from dist.ipfs.io (opens new window)
wget https://dist.ipfs.io/go-ipfs/v0.12.2/go-ipfs_v0.12.2_linux-amd64.tar.gz
# 2 - Unzip the file (optional step is to delete then the tar.gz file)
tar -xvzf go-ipfs_v0.12.2_linux-amd64.tar.gz
# 3 - Move into the go-ipfs folder and run the install script
cd go-ipfs
sudo bash install.sh
# 4 - Test that IPFS has installed correctly:
ipfs --version
```

* Open the necessary port on `UFW` as described [here](https://docs.ipfs.io/how-to/nat-configuration/#port-forwarding):
```bash
sudo ufw allow 4001/tcp
sudo ufw status # to check
```

* Adjust the maximum buffer size (from 300k to to 2500k) since when launching the daemon an error message would be shown complaining about the buffer size:
```bash
sudo sysctl -w net.core.rmem_max=2500000
```

* Adjust the default [maximum disk storage for data retrieved from other peer](https://github.com/ipfs/go-ipfs/blob/v0.12.2/docs/config.md#datastorestoragemax)
```bash
# Adjust IPFS repo size from default 10GB to 50MB
ipfs config Datastore.StorageMax 50MB
```

* [Initialize the daemon](https://docs.ipfs.io/how-to/command-line-quick-start/#initialize-the-repository)
```bash
ipfs init --profile server
```

* Let's try some commands to make sure ipfs works:
```bash
# Test that ipfs is working, by requesting and viewing the readme file
ipfs cat /ipfs/QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG/readme
# You can explore other objects in the repository. In particular, the `quick-start` directory which shows example commands to try
ipfs cat /ipfs/QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG/quick-start
# Download a JPG file to locally
ipfs cat /ipfs/QmSgvgwxZGaBLqkGyWemEDqikCqU52XxsYLKtdy3vGZ8uq > ./spaceship-launch.jpg
```

* Launch the daemon online to do some more commands
```bash
# Launch deamon, take note of the PID, to shutdown the service use `kill <PID>`
ipfs daemon &
# Check peers online
ipfs swarm peers
```

* It works! Now just need to create a service to make sure IPFS is running all the time, even after reboot. Use command `sudo vi /etc/systemd/system/ipfs.service` and enter
```bash
[Unit]
Description=IPFS Daemon
[Service]
Type=simple
ExecStart=ipfs daemon --enable-gc
Group=pvaladar
Restart=always
Environment="IPFS_PATH=/home/pvaladar/.ipfs"
[Install]
WantedBy=multi-user.target
```

* Start and enable IPFS service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable ipfs
sudo systemctl start ipfs
```

* Check that IPFS is active and enabled:
```bash
sudo systemctl status ipfs
```

## 4.3 Final checks

> • Set up a service of your choice that you think is useful (NGINX / Apache2 excluded!).
As final check, make sure that `nginx` and `apache` are not installed before making the final hash
```bash
dpkg -l | grep ngix
dpkg -l | grep apache
```

# 5 - Preparation for Defense

## 5.1 - Operating System (`Debian`), Package Managers (`aptitude` vs `apt`) and `AppArmor`

> You must choose as an operating system either the latest stable version of Debian (no testing/unstable), or the latest stable version of CentOS. **Debian is highly recommended if you are new to system administration.**
>
> Setting up CentOS is quite complex. Therefore, you don’t have to set up KDump. However, SELinux must be running at startup and its configuration has to be adapted for the project’s needs. 

* Explanation about differences between Debian and CentOS [here](https://www.fosslinux.com/49958/debian-vs-centos.htm)


> **AppArmor for Debian must be running at startup too.**
* Check that `AppArmor` is running with command `sudo aa-status`

> **During the defense, you will be asked a few questions about the operating system you chose.** For instance, you should **know the differences between `aptitude` and `apt`**, or what SELinux or **`AppArmor` is**. In short, understand what you use!
* As noted on subject *Debian is highly recommended if you are new to system administration* and *Setting up CentOS is quite complex.*

* Differences between `aptitude` and `apt` [here](https://www.tecmint.com/difference-between-apt-and-aptitude/). Both are frontends of `dpkg` (package manager for Debian) that has the limitation of not handling dependencies, being `apt` older and with less features than `aptitude` (a fancy interactive manager). More info [here](https://askubuntu.com/questions/309113/what-is-the-difference-between-dpkg-and-aptitude-apt-get), [here](https://en.wikipedia.org/wiki/APT_(software)) and [here](https://en.wikipedia.org/wiki/Aptitude_(software)).

* Explanation about [AppArmor](https://apparmor.net/):

*Linux kernel security module | AppArmor is an effective and easy-to-use Linux application security system. AppArmor proactively protects the operating system and applications from external or internal threats, even zero-day attacks, by enforcing good behavior and preventing both known and unknown application flaws from being exploited.*

 ## 5.2 - `ssh`
 
> For security reasons, it must not be possible to connect using SSH as root.
> The use of SSH will be tested during the defense by setting up a new account. You must therefore understand how it works.
* On server machine type `ip addr` to know the <ip address>.
* Attempt to login as `root` to see the error message: `ssh root@<ip address> -p 4242`

## 5.3 - `ufw`

> Your firewall must be active when you launch your virtual machine.
* After reboot check it is active with `sudo ufw status` and `sudo systemctl status ufw`

## 5.4 - Users and Groups

> During the defense, you will have to create a new user and assign it to a group.
```bash
# Create new user
sudo adduser <username>
# Add new group
sudo groupadd <group_name>
# Add user to a group
sudo adduser <username> <group_name>
# Current user
whoiam
# See current user groups
groups
```

## 5.5 - `cron`

> During the defense, you will be asked to explain how this script works. You will also have to interrupt it without modifying it. Take a look at cron.
* To interrupt type `sudo systemctl stop cron`. To revert, either type `sudo systemctl start cron` or `sudo reboot`.

# 6 - Resources

* [VirtualBox Manual](https://www.virtualbox.org/manual/)
* [VirtualBox Networking Reference](https://blogs.oracle.com/scoter/post/oracle-vm-virtualbox-networking-options-and-how-to-manage-them)
* [man pages](https://man7.org/linux/man-pages)
* [crontab guru](https://crontab.guru/)
* [RamonLucio Guide](https://github.com/RamonLucio/Born2beRoot)
* [clemedon Guide](https://clemedon.github.io/born2beroot_42/)
* [ZakariaMahmoud Guide](https://github.com/ZakariaMahmoud/Born2beRoot_101)
* [baigal Guide](https://baigal.medium.com/born2beroot-e6e26dfb50ac)
* [Headlighter Guide](https://github.com/HEADLIGHTER/Born2BeRoot-42)
* [Born2BeRoot Correction](https://github.com/sltcestloic/born2beroot_correction/blob/master/correction_born2beroot.pdf)
