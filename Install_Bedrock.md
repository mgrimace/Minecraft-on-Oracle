# Install Minecraft Bedrock directly on the server

If you prefer to use a WebUI to deploy and manage multiple Minecraft servers, go [here](Install_Crafty.md) instead to install Crafty first.

## Installing Minecraft Bedrock

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

Once inside the server screen, you can add your gamertag as an operator via `op [gamertag]`. It appears you have to actually be connected to the server from your game to do so. Note the `xuid` in case this doesn't work.

If it didn't work, detach the screen with ctrl+a, d. Then,`cd` to the servername folder and find permissions.json. 
`nano permissions.json` to edit it, and add:

```
[
{
"permission": "operator",
"xuid": "[your gamertag's XUID from above]"
}
]

```

You can also add other levels via:

```
[
{
"permission": "operator",
"xuid": "[your xuid]"
},
{
"permission": "member",
"xuid": "[your friend's xuid]"
},
{
"permission": "visitor",
"xuid": "[someone else's xuid]"
}
]

```

Again, all these xuid's can be found by attaching the server screen via `screen -r [servername]` and connecting a user to the server. 

## Accessing the server (in the game)

Go to multiplayer, add server, and use the Ubuntu server's public IPv4 address. 

### Connect from Xbox

If you need to connect via console, there's some addtional instructions, go here [next](Connect_Xbox_to_server.md)

