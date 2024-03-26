# Installing Crafty

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

Disable IPTables (Oracle has it's own firewall)

```bash
sudo iptables -P INPUT ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -F
sudo netfilter-persistent save
```

note the `sudo netfilter-persistent save` should save an empty ruleset to disk so it will be reloaded on reboot, otherwise you'd need to re-run the commands each time the server reboots.

## Forward ports in Oracle

### Network list ports

1. After it’s created, go to the instance details, find “Primary VNIC” section, and open the subnet link (or create a new one).

2. Open Default Security List (or create a new one if one doesn’t exist yet)

3. Add Ingress Rules to open TCP ports 8443 for Crafty WebUI. Use CIDR for Source Type, 0.0.0.0/0 for Source CIDR, all for source, 19132 for Destination port. 


### Network security group ports

1. Then, go to networking, virtual cloud networks. Select your compartmen on the sidebar, then select your vcn from the list.

2. In the VCN settings, on the resources sidebar, select Network Security Groups, select the one we created earlier (or create a new one), and add the same rules exactly as above.


## (Optional) Import your old Minecraft server

If you had a previously created a Bedrock Minecraft, you can back it up and import it into crafy.

We need to copy our old server to a new location so it can be imported

```bash
cd /var/opt/minecraft/crafty
sudo mkdir imports
sudo cp -a /home/ubuntu/minecraftbe/[servername]/. /var/opt/minecraft/crafty/imports/[servername]
sudo chown -R crafty:crafty /var/opt/minecraft
sudo chmod -R 2775 /var/opt/minecraft
```

Connect to the webui by going to `https://IP:8443`,

 create new server, be sure to select the bedrock toggle, then select import from `/var/opt/minecraft/crafty/imports/[servername]` with executable `bedrock_server`

To change properties, go to the server in the crafty webui and select files then change server.properties

## Create a server

Use Crafty to create, deploy, and manage servers per their [documentation](https://docs.craftycontrol.com/pages/getting-started/access/)

### Accessing the server (in the game)

Go to multiplayer, add server, and use the Ubuntu server's public IPv4 address and port you assigned. 

### Connect from Xbox

If you need to connect via console, there's some addtional instructions, go here [next](Connect_Xbox_to_server.md)

### Setup a Crossplay enabled server

As well, you can setup a cross-play compatible server, go here for [instructions](server_crossplay.md)

