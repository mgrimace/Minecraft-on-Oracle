# Installing Crafty

## Oracle setup
If you haven't already done so, make sure you have forwarded ports in both the OCI interface, and on your Ubuntu server. Instructions to do so are found on [main page](README.md#forward-ports). After ensuring all the appropriate ports are opened (e.g., specifically, 8443 for the Crafty webUI), proceed with the installation of Crafty.

## Previous Minecraft Install?
If you previously set up a bedrock server, you'll need to stop it first 

```bash
sudo systemctl stop [servername]
sudo systemctl disable [servername]
```

## Install Crafty

```bash
sudo apt update && sudo apt upgrade && sudo apt install git
git clone https://gitlab.com/crafty-controller/crafty-installer-4.0.git && \
 cd crafty-installer-4.0 && \
 sudo ./install_crafty.sh
```

Agree to creating a servce during the install

```bash
sudo systemctl start crafty
sudo systemctl enable crafty
```

## (Optional) Import your old Minecraft server

1. If you had a previously created a Bedrock Minecraft, you can back it up and import it into crafy.

2. We need to copy our old server to a new location so it can be imported


```bash
cd /var/opt/minecraft/crafty
sudo mkdir imports
sudo cp -a /home/ubuntu/minecraftbe/[servername]/. /var/opt/minecraft/crafty/imports/[servername]
sudo chown -R crafty:crafty /var/opt/minecraft
sudo chmod -R 2775 /var/opt/minecraft
```

3. Connect to the webui by going to `https://IP:8443`,
4. create new server, be sure to select the bedrock toggle, then select import from `/var/opt/minecraft/crafty/imports/[servername]` with executable `bedrock_server`
5. To change properties, go to the server in the crafty webui and select files then change server.properties

## Create a server

- Use Crafty to create, deploy, and manage servers per their [documentation](https://docs.craftycontrol.com/pages/getting-started/access/)


### Accessing the server (in the game)

- Go to multiplayer, add server, and use the Ubuntu server's public IPv4 address and port you assigned. 


# Next steps

Return to the [main page](README.md)

### Connect from Xbox

- If you need to connect via console, there's some addtional instructions, go here [next](Connect_Xbox_to_server.md)


### Setup a Crossplay enabled server

- As well, you can setup a cross-play compatible server, go here for [instructions](server_crossplay.md)
