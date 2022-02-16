Follow this in your root terminal after using SSH

sudo timedatectl set-timezone America/New_York
sudo apt update && sudo apt upgrade -y
sudo apt-get install unattended-upgrades apt-listchanges -y
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
