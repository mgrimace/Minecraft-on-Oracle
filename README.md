# Create a Minecraft server on Oracle Free Tier
Instructions to create a free Minecraft server on an Oracle free tier on Ubuntu 22.04 with a 2 OCPU and 12 gb ram Ampere virtual machine.
This guide assumes you have already created a free account in Oracle, and have the knowledge/ability to connect to a server via SSH.

## Create a new instance

Select Ubuntu 20.04. We're using the full version, not minimal, so that we can select an Ampere shape. This will allow us to have more cores and ram than the default Ubuntu image, which only has 1gb ram. 

Select shape Ampere. I used 2 OCPU and 12 gb ram (this is half of the free tier allowance)

Make sure that assign public IPv4 is checked in network, and save your public and private SSH keys.

Create the instance. 

## Forward ports

### Network list ports

After it’s created, go to the instance details, find “Primary VNIC” section, and open the subnet link (or create a new one).

Open Default Security List (or create a new one if one doesn’t exist yet)

Add Ingress Rules to open TCP/UDP ports 19132 for Bedrock and 25565 for Java edition (or both). Use CIDR for Source Type, 0.0.0.0/0 for Source CIDR, 19132 for Destination port. Repeat for TCP. Repeat for 25565 if planning to use Java edition.

### Network security group ports

Then, go to networking, virtual cloud networks. Select your compartmen on the sidebar, then select your vcn from the list.

In the VCN settings, on the resources sidebar, select Network Security Groups, create new, and add the same rules exactly as above.

## Setup the server

SSH to your server: `ssh ubuntu@ip_address`

Upgrade all packages:

```bash
sudo apt-get update
sudo apt-get upgrade
```

### Open ports on the server

Let’s reset the firewall rules and open the ssh and Minecraft ports:

```bash
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -F
sudo iptables-save
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 19132/udp
sudo ufw allow 19132/tcp
sudo ufw allow 25565/udp
sudo ufw allow 25565/tcp
sudo ufw enable
sudo ufw status
```

### Install Bedrock 

We’ll use this script to setup the server: https://github.com/TheRemote/MinecraftBedrockServer

First, we need to install some dependencies:

```bash
sudo wget https://ryanfortner.github.io/box64-debs/box64.list -O /etc/apt/sources.list.d/box64.list
wget -qO- https://ryanfortner.github.io/box64-debs/KEY.gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/box64-debs-archive-keyring.gpg 
sudo apt update && sudo apt install box64-rpi4arm64 -y
```

Then, install the script via:

```bash
curl https://raw.githubusercontent.com/TheRemote/MinecraftBedrockServer/master/SetupMinecraft.sh | bash
```

Note the prompts to name your server (note: don't call it *minecraft* or *minecraftbedrock*), select the option to start automatically on server boot, select the option to restart automatically at 4:00 am (which will automatically take a backup of your server for you)

You can use the following commands to start/restart the service (you specified the service name when you ran the installation script):

```bash
sudo systemctl stop [minecraft service name set]
sudo systemctl start [minecraft service name set]
sudo systemctl restart [minecraft service name set]
```

You can set up multiple servers this way, for example, re-run the script and give it a different name.

#### Accessing the server (on the backend)

The actual Bedrock server itself is running on Ubuntu in the background using 'screens'. You can access the server process running in the background by connecting via SSH to your Ubuntu instance, and using screens: 

- `screen -ls` lists the running server names
- `screen -r [server name]` will connect you to the server. You can use the console to change settings, etc.
- `ctrl+a`, the the letter `d` on your keyboard will detach the screen to keep the server running in the background when you're done.

#### Add yourself as an operator

Once inside the server screen, you can add your gamertag as an operator via `op [gamertag]`. It appears you have to actually be connected to the server from your game to do so. 

## Accessing the server (in the game)

Go to multiplayer, add server, and use the Ubuntu server's public IPv4 address. 
