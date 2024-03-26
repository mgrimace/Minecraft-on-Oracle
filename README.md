# Create a Minecraft server on Oracle Free Tier

These are instructions to create a free Minecraft server on an Oracle free tier on Ubuntu 22.04 with a 4 OCPU and 24 gb ram Ampere virtual machine.

This guide assumes you have already created a free account in Oracle, and have the knowledge/ability to connect to a server via SSH.

This guide also provides steps to connect an Xbox Series Console to the custom server. As well, I've added intructions to optionally install [Crafty](https://craftycontrol.com) a neat free server dashboard to make it easier to deploy and manage your Minecraft server(s). Recently, I added isntructions for hosting a cross-platform server via Crafty to enable players on both Java and Bedrock to play together

1. [Create your Oracle server](#Getting_started)
2. Install Minecraft on the server
   1. [Option 1](Install_Bedrock.md): install Minecraft Bedrock direclty on the server
   2. [Option 2](Install_Crafty.md): install Crafty, a GUI for deploying and managing multiple minecraft servers (my preferred option) 

3. [Connect to the server on Xbox](Connect_Xbox_to_server.md)
4. [Other Oracle settings: increase storage, reserve static IP, add cloudflare](Oracle_additional_settings.md)
5. [Optional: create a server with crossplay enabled for java and bedrock](server_crossplay.md)

# Getting started

## Create a new instance

1. Select Ubuntu 20.04. We're using the full version, not minimal, so that we can select an Ampere shape. This will allow us to have more cores and ram than the default Ubuntu image, which only has 1gb ram. 

2. Select shape Ampere. ~~I used 2 OCPU and 12 gb ram (this is half of the free tier allowance)~~ I switched to 4 OCPU and 24gb ram, which is the max for free tier. Scale down if necessary.

3. Make sure that assign public IPv4 is checked in network, and save your public and private SSH keys.

4. Create the instance. 


## Forward ports

### Network list ports

1. After it’s created, go to the instance details, find “Primary VNIC” section, and open the subnet link (or create a new one).

2. Open Default Security List (or create a new one if one doesn’t exist yet)

3. Add Ingress Rules to open TCP/UDP ports 19132 for Bedrock and 25565 for Java edition (or both). Use CIDR for Source Type, 0.0.0.0/0 for Source CIDR, 19132 for Destination port. Repeat for TCP. Repeat for 25565 if planning to use Java edition.


### Network security group ports

1. Then, go to networking, virtual cloud networks. Select your compartmen on the sidebar, then select your vcn from the list.

2. In the VCN settings, on the resources sidebar, select Network Security Groups, create new, and add the same rules exactly as above.


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

## You have two options for installing Minecraft 

1. Install Minecraft Bedrock directly on the server to play on PC and Xbox, proceed [here](Install_Bedrock.md)
2. Install Crafty, a server manager to manage and deploy different versions of minecraft, proceed [here](Install_Crafty.md)





