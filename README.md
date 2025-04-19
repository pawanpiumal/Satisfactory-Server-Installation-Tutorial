# Satisfactory-Server-Installation-Tutorial

1. Update and Install required software

```
sudo add-apt-repository multiverse; sudo dpkg --add-architecture i386; sudo apt update
sudo apt install steamcmd
sudo apt-get install unzip
```

2. Download DepotDownloader

```
wget "https://github.com/SteamRE/DepotDownloader/releases/download/DepotDownloader_2.7.1/DepotDownloader-linux-x64.zip"
unzip DepotDownloader-linux-x64.zip
```

3. Use SteamDB to get the manifest ID of the version of satisfactory dedicated server you want.

4. Use the following to download that version

```
./DepotDownloader -app 1690800 -depot 1690802 -manifest 1910179703516567959
```

5. Launch the server

```
bash FactoryServer.sh
```

6. Open port 7777 in the network console.

7. Configure server settings to auto launch the Satisfactory server when server restarts.

8. Run the following to create the service file.

```
sudo apt install vim
cd /etc/systemd/system/
sudo touch satisfactory.service
```
 
9. Change permissions to edit the file. Change username. (user)

```
sudo chown user:user satisfactory.service
sudo chmod 775 satisfactory.service
```

10. Edit the file with vim.

```
vim satisfactory.service
```

11. Add the following code. Replace user with Username.

```
[Unit]
Description=Satisfactory dedicated server
Wants=network-online.target
After=syslog.target network.target nss-lookup.target network-online.target

[Service]
Environment="LD_LIBRARY_PATH=./linux64"
ExecStart=/home/user/Satisfactory/FactoryServer.sh -ServerQueryPort=15777 -BeaconPort=15000 -Port=7777 -log -unattended -multihome=0.0.0.0
User=user
Group=user
StandardOutput=journal
Restart=on-failure
WorkingDirectory=/home/user

[Install]
WantedBy=multi-user.target
```

12. Start the service.

```
sudo systemctl daemon-reload
sudo systemctl enable satisfactory
sudo systemctl start satisfactory
sudo systemctl status satisfactory
```
