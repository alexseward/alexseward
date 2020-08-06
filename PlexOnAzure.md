# Running a Plex media server on Azure

For years I was dedicated to my craft of buying records of vinyl, ripping the songs to WAV and Djing alongside my digital collection of music compiled over years from various sources. More recently, I've found myself almost entirely converted to the use of Spotify! However, some of my more obscure soundcloud downloads and vinyl rips are missing from Spotify and so an Azure based Plex server, and the Plex app on my laptop and phone are the perfect solution.

I followed the steps from this video https://youtu.be/ylklaOXz8Fc with a few tweaks along the way.

## Virtual Machine
1. Download Plex Media Server from https://www.plex.tv/media-server-downloads/. In this tutorial, I will use a Linux VM and so downloaded the Linux version of Plex.
2. Create a resource group. Mine is called 'Plex'.
3. Inside 'Plex' RG, create a storage account. Mine is called 'alexplex'.
4. Once created, inside your storage account e.g. 'alexplex', click File shares (under File Service) and add a File share. Mine is called 'plex' and I set the Quota as 1024.
5. Inside 'Plex' RG, create a virtual network. Mine is called 'plex-vnet'.
6. Inside 'Plex' RG, create a Linux virtual machine. I used the most recent Ubuntu Server OS image available on the Azure Marketplace. I used a B1s VM and mine is called 'plex-LVM1'.
7. Link the previously created storage to the VM.
8. Click on Network Security Group, and add an inbound rule for 1001 port.
9. Validate and Deploy VM. Once deployed, copy the public IP address for the VM.

## PuTTy
1. As I'm using a Linux VM, I am going to use PuTTy. Download PuTTy from https://www.ssh.com/ssh/putty/linux/
2. Open PuTTy and paste your VM's IP address into the Host Name field.
3. Click the plus next to SSH on the left panel, then click Tunnels.
4. Enter 8888 into the Source port field and localhost:32400 in Destination field.
5. Click Session on the left panel, save this PuTTy configuration, and then click Open.
6. Enter your password to log in to the VM.
7. Enter the following commands:
```
sudo mkdir /downloads
cd /downloads/
sudo wget https://downloads.plex.tv/plex-media-server-new/1.20.0.3181-0800642ec/debian/plexmediaserver_1.20.0.3181-0800642ec_amd64.deb
sudo dpkg -i plexmediaserver_1.20.0.3181-0800642ec_amd64.deb
```

## Plex
1. Open a browser window and type http://localhost:8888/web into the address bar.
2. Log in with your Plex account and go through the steps to setup the Plex Server.
3. When at the main Plex homescreen and ready to add media, return to the Azure Portal.
4. Find your File share e.g. 'plex' and click Connect.
5. Copy the command for Linux.
6. Open PuTTy and run this command to mount your file share to a directory on the VM.
7. Return to Plex, and click to Add Library. Browse for Media Folder and select the folder on your file share.

## Azure Storage Explorer
1. Open Azure Storage Explorer. You may need to install this if it is not installed on your local machine.
2. Add your Azure account and sign in
3. Find your File share e.g. 'plex' and start to add your media from your local machine to the File share
4. Open Plex and see your files begin to show :)








