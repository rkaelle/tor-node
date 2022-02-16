This guide is for a middle relay node.

Follow this in your root terminal after using SSH

    sudo timedatectl set-timezone America/New_York
    sudo apt update && sudo apt upgrade -y
    sudo apt-get install unattended-upgrades apt-listchanges -y
    sudo nano /etc/apt/apt.conf.d/50unattended-upgrades


Next...

uncomment the following lines in the opened file:

    "${distro_id}:${distro_codename}-updates";
    Unattended-Upgrade::Remove-Unused-Kernel-Packages "true";
    Unattended-Upgrade::Remove-Unused-Dependencies "true";
    Unattended-Upgrade::Automatic-Reboot "true";
    Unattended-Upgrade::Automatic-Reboot-Time "17:00";
    Unattended-Upgrade::Automatic-Reboot-WithUsers "true";

Open: 

    sudo nano /etc/apt/apt.conf.d/20auto-upgrades
    
Replace in the opened file:

    APT::Periodic::Update-Package-Lists "1";
    APT::Periodic::AutocleanInterval "5";
    APT::Periodic::Unattended-Upgrade "1";
    APT::Periodic::Verbose "1";
    
    
test for errors:

    sudo unattended-upgrades --dry-run
    sudo cat /var/log/unattended-upgrades/unattended-upgrades.log
    
Activate Unattended upgrades:

    sudo dpkg-reconfigure -plow unattended-upgrades
    
Install and open:

    sudo apt install apt-transport-https -y
    sudo nano /etc/apt/sources.list
    
Append in the open file:
    
    deb [signed-by=/usr/share/keyrings/tor-archive-keyring.gpg] https://deb.torproject.org/torproject.org focal main
    deb-src [signed-by=/usr/share/keyrings/tor-archive-keyring.gpg] https://deb.torproject.org/torproject.org focal main
    
Next:

    wget -O- https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | gpg --dearmor | tee /usr/share/keyrings/tor-archive-keyring.gpg >/dev/null
    apt update && apt upgrade -y
    apt install tor deb.torproject.org-keyring -y
    sudo nano /etc/tor/torrc
    
Use ^K to delete each line of the open file and replace with:

    Nickname coolnode
    ORPort 443
    ExitRelay 0
    SocksPort 0
    ControlSocket 0
    ContactInfo your@email.com
    
To get up and running:

    sudo systemctl restart tor@default
    sudo ufw enable
    sudo ufw allow 443
    sudo ufw status
    
    
To set max values:

    sudo nano /etc/tor/torrc

Append in file:

    AccountingStart day 0:00
    AccountingMax 50 GBytes
    RelayBandwidthRate 25 MBytes
    RelayBandwidthBurst 100 MBytes
    
    
Installing nyx (the application used to monitor your node):

    sudo apt-get install nyx -y
    sudo nano /etc/tor/torrc
    
In the open file append to the end:

    ControlPort 9051
    CookieAuthentication 1
    
Starting the service!!

    sudo systemctl restart tor@default
    nyx
