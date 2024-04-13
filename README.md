# Raspberry-Pi-5-Proxmox-8.1-Installation

# Preparing your Raspberry Pi for Proxmox
1. Our first task before installing Proxmox onto the Raspberry Pi is to update the package list cache and upgrade any out-of-date packages.
You can perform both tasks by using the following two commands within the terminal.

`sudo apt update
sudo apt upgrade`
3. Your next step is to ensure that curl is installed on your Pi. We will be using curl to grab the GPG key for the Proxmox ports repository that we will be relying on.
You can install this package by using the following command within the terminal.
sudo apt install curl
4. Before proceeding with this tutorial, you must set up your Raspberry Pi to use a static IP address.
The best way to do this is using DHCP reservation in your router. However, we have a guide that shows you how to do this through your Raspberry Pi if you don’t have access to your router.

# Modifying your Hosts File for Proxmox
4. After setting up a static IP address, we must now edit the hosts file. Proxmox expects your hostname to point to the IP address of your Raspberry Pi.
You can begin editing the hosts file on the Raspberry Pi using the nano text editor by running the following command.
sudo nano /etc/hosts
5. With the hosts file open, you should see a line similar to the one below. Our hostname is set to the default “raspberrypi“, but yours might differ.
127.0.0.1            raspberrypi
After finding this line, you will want to replace “127.0.0.1” with the local IP address of your Raspberry Pi.
In our case, our Pi’s IP is “192.168.0.32” so we changed the line to look like the following:
192.168.0.32            raspberrypi
6. After making these changes, save and quit by pressing CTRL + X, Y, and then the ENTER key.
7. To verify that our changes are working, we can use the hostname command to output the IP address of our Raspberry Pi.
hostname --ip-address
If you have configured everything properly, the static IP of your Pi should be returned.
192.168.0.32

# Set a Password for the root User
8. The next thing we must do to use Proxmox on our Raspberry Pi is set a password for the “root” user. If we don’t set a password for this user we will be unable to login through the Proxmox web interface.
You can set a password for the root user using the command below within the terminal.
sudo passwd root
9. To set a password, you must enter it twice, as shown below. We recommend that you set a strong password for this.
A password manager such as NordPass (Affiliate Link) makes generating and storing strong passwords significantly easier.
New password:
Retype new password:

# Adding the Proxmox Port Repository to your Raspberry Pi
10. We are finally at the stage where we can begin adding the Proxmox Ports repository to our Raspberry Pi. This repository is managed by a third-party but allows us to install versions compiled for the Raspberry Pi’s hardware.
11. Our first step in this process is to add the GPG key for the repository. This key helps verify that the packages legitimately come from the Proxmox ports repository.
curl -L https://mirrors.apqa.cn/proxmox/debian/pveport.gpg | sudo tee /usr/share/keyrings/pveport.gpg >/dev/null
11. With the GPG key added, we can now add the repository itself to our sources list.
Use the command below to save the repository link into a file called “pveport.list“.
echo "deb [deb=arm64 signed-by=/usr/share/keyrings/pveport.gpg] https://mirrors.apqa.cn/proxmox/debian/pve bookworm port" | sudo tee  /etc/apt/sources.list.d/pveport.list
12. Since we made changes to the sources list, we must update the package list cache again by running the following command.
If we don’t do this, the package manager will be unaware of the repository we just added.
sudo apt update

# Installing Proxmox to the Raspberry Pi from the Repository
13. With everything in place, we can finally begin installing Proxmox on the Raspberry Pi.
The first package that must be installed on our system is “ifupdown2“. Proxmox uses this package to handle the network.
sudo apt install ifupdown2
14. Finally, you can install Proxmox VE and a few required packages by typing the following command in the terminal.
This installation process can take a few minutes, so be prepared to wait a few minutes. Additionally, there are a few prompts you will have to go through during installation.
sudo apt install proxmox-ve postfix open-iscsi pve-edk2-firmware-aarch64
15. During installation, you will be asked to configure Postfix. You can navigate this menu using the ARROW keys to move and ENTER to select.
If you are unsure what you are doing here, select the “Local only” option.
16. Next, you will be asked to set the mail name for Postfix to utilize.
Again, if you don’t know what you are doing here, leave the default name and press ENTER to continue.

# Accessing the Proxmox Web Interface
17. Once Proxmox finishes installing to your Raspberry Pi, it should be safe to access its web interface.
All you need to access the web interface is the IP address of your Raspberry Pi.
hostname -I
18. With your IP address in hand, you will want to go to the following address in your favorite web browser.
Ensure that you replace “<IPADDRESS>” with the IP of your Raspberry Pi.
https://<IPADDRESS>:8006
19. With the Proxmox web interface now open in your web browser, you must log in to your account.
20. Start by typing in the username as “root” and then use the password you set earlier for this user.
Once you have filled in your information, click the “Login” button.
20. You should now have access to the Proxmox web interface.
You can begin to set up new VMs through this interface as well as configure Proxmox VE itself.

Thanks to: https://pimylifeup.com/raspberry-pi-proxmox/
