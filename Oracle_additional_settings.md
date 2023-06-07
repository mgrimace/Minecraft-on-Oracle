# Other Oracle options

## Create an elastic (static) IP

- Go to oracle, networking, IP management, Reserve public IP
- create a new reserved IP (note, it won't be the same as your curren ip!)
- then go back to compute, instances, and on the sidebar, see resources, attached VNICs
- click your host name, go to edit, release the current IP, then save, then go back to edit, and select your reserved IP

## Expand your server's storage

- You get up to 200 gb storage free, and if you're like me you maybe forgot to expand the storage when you created the instance which will have about 47 gb by default
- Go to orace, stoarge boot volumes, edit, and set the new storage to a higher number (e.g., 128 gb), wait
- It will give you commands you have to set in your terminal
- SSH to your ubuntu terminal and paste the commands

```bash
#1. Change to superuser 

sudo su - 

#2. After increasing the boot volume size in oracle dash, you'll get a rescan command that will look something like this below 

sudo dd iflag=direct if=/dev/oracleoci/oraclevda of=/dev/null count=1
echo "1" | sudo tee /sys/class/block/`readlink /dev/oracleoci/oraclevda | cut -d'/' -f 2`/device/rescan

#3. grow the partition (note: there is a space between sda and 1 here) **

growpart /dev/sda 1 

#4. resize the filesystem (note: there is no space on sda1 this time**

resize2fs /dev/sda1

#you can confirm by checking disk free, with the command below **

df -h

```

## Add a subdomain (and Cloudflare) for Crafty and your server

This assumes you already have a domain setup with Cloudflare using strict SSL (e.g., for your [homelab](https://github.com/mgrimace/Homelab)), and you want to add a Minecraft/Crafty subdomain (e.g., crafty.mydomain.com) but use Oracle to keep it separate from your home. You can view my instructions for setting up a domain and cloudflare from scratch on my [homelab](https://github.com/mgrimace/Homelab) repo under network.

### Add your subdomain 

- Go to Cloudflare, DNS, records
- Add a record with Type: A, name: crafty, IPv4: OracleIP, proxy enabled

- Add another record, similar, but with the name minecraft, mc, or anything you want to call the actual server

### Make a strict -> full SSL exception for Oracle

- Go to SSL/TLS, overview

- select 'create a configuration rule to customize these settings by hostname'

- call it 'oracle override' or something similar

- If, select field: hostname, operator: starts with, value: crafty, select 'or' and add another similar line with minecraft or mc or whatever you use for your server 

- Scroll down to the end, and select SSL, +add, and choose Full as your encryption, save and enable.

Visit Crafty at crafty.mydomain.com:8443 and access your minecraft server using minecraft.mydomain.com and its port (or minecraft1, mc1, etc.)
