# Create a Minecraft server on Oracle Free Tier

I created this guide as a father looking for a way for my son to play Minecraft with his friends. 

These are instructions to create a free, always-on Minecraft server using Oracle's always-free tier. Using this guide we will create a Ubuntu 22.04 server with a 4 OCPU and 24 gb ram Ampere virtual machine, which is plenty to run multiple Minecraft servers.

This guide assumes you have already created a free account in Oracle, and have the knowledge/ability to connect to a server via SSH.

This guide also provides steps to connect an Xbox Console to the custom server. As well, I've added intructions to  install [Crafty](https://craftycontrol.com) a neat free server dashboard to make it easier to deploy and manage your Minecraft server(s). Recently, I added instructions for hosting a cross-platform server via Crafty to enable players on both Java and Bedrock players to play together.

## Table of Contents

1. [Create your Oracle server](#Getting_started)
2. Install Minecraft on the server
   1. [Option 1](Install_Bedrock.md): install Minecraft Bedrock direclty on the server
   2. [Option 2](Install_Crafty.md): install Crafty, a GUI for deploying and managing multiple minecraft servers (my preferred option) 
3. [Connect to the server on Xbox](Connect_Xbox_to_server.md)
4. [Other Oracle settings](Oracle_additional_settings.md): increase storage, reserve static IP, add cloudflare
5. [Optional](server_crossplay.md): create a server with crossplay enabled for java and bedrock
6. [Support this project](#Support_this_project)

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

3. Add Ingress Rules to open UDP ports 19132 for Bedrock, both TCP and UDP ports 25565 for Java edition (or do them both while you're here), and TCP port 8443 for Crafty's webUI. Use CIDR for Source Type, 0.0.0.0/0 for Source CIDR, and the specific port noted for each for the Destination port. 


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
sudo netfilter-persistent save
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw limit 22/tcp
sudo ufw allow 19132/udp
sudo ufw allow 19134/udp
sudo ufw allow 25565
sudo ufw allow 8443/tcp
sudo ufw enable
sudo ufw status
```

**Notes:** 

- `netfilter-persistent` should save an empty ruleset to disk so it will be reloaded on reboot, otherwise you'd need to re-run the commands each time the server reboots.
- For bedrock I'm opening ports 19132/upd and 19134/upd which will allow me to have two servers, you can open more as needed if you add additional servers
- For Java I'm opening 25565
- Port 8443 is what we'll use to access the Crafty webUI
- I'm using `limit` on 22 to prevent brute force connections to SSH

## You now have two options for installing Minecraft 

1. Install Minecraft Bedrock directly on the server to play on PC and Xbox, proceed [here](Install_Bedrock.md)
2. Install Crafty, my preferred option, to manage and deploy different versions of Minecraft. To do so proceed [here](Install_Crafty.md)

## Support this project

If you found this guide helpful, please consider buying me a coffee by clicking the link below. I'll do my best to keep this guide up to date and as user-friendly as possible. Thank you and take good care!

[![Donate](https://camo.githubusercontent.com/0283ea90498d8ea623c07906a5e07e9e6c2a5eaa6911d52033687c60cfa8d22f/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d50617950616c2d677265656e2e737667)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=R4QX73RWYB3ZA)

