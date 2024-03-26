# Creating a crossplay-enabled Minecraft server in Crafty

You can create a Minecraft server that will allow people using both Java and Bedrock editions to play together.

## Create a Paper server in Crafty

1. Go to your Crafty UI, select `create new server`
2. - Server Type: Servers
   - Server Select Paper
   - Server Version: 1.20.4 (or whatever the current server version is)
3. Give it a name, and select 'Build Server!'
4. Start the server, then once it's finished loading up and installing, stop the server.

## Get the required plugins

1. Go to https://geysermc.org/download 
2. Download the Spigot/Paper version of Geyser, and Floodgate. They should download to your computer as .jar files

## Install the plugins on your server

1. Go back to your Crafty UI, select your server, then select 'files'
2. Right-click on the 'plugins' folder, and upload the two .jar files you downloaded in the previous step
3. If you get stuck at 100%, log out of Crafty, log back in and try again, it's a known issue.
4. Restart the server again, wait for the plugins to install, then stop the server.

## Configure Geyser

1. Go back to files, you should now see a folder in /plugins, called Geyser-Spigot, open the folder, and select config.yml
2. You should see 'Editing file config.yml' now, go to line 19, or the line starting `port:` and change it to Paper server port, default 25565 so it reads `port:25565`
3. Feel free to change the `motd` or 'message of the day' for visitors, and no other changes are needed, so hit 'save'

## Optional (recommended), enable whitelist

This will prevent randoms and bots from joining your game

1. While in files, select the file server.properties, and in the edit menu change `white-list=true` on line 48, and `enforce-whitelist=true` on line 58
2. Now, when people attempt to connect, it will say 'You are not added to the whitelist', and you must manually add or whitelist people
3. You can also manage players in the player management menu in Crafty for this server (e.g., kick, OP, etc)

### Add people to the whitelist

1. If you know their Minecraft usernames, go to >_Terminal and enter `whitelist add .[username]` the `.` before the username is important
2. If you don't know their Minecrafty username, ask your friend to connect, then watch the terminal, you should see Player connect with username but is not whitelisted. You can add them now with this username 

### Connect from Xbox

If you need to connect via console, there's some addtional instructions, go here [next](Connect_Xbox_to_server.md)

## Support this project

If you found this guide helpful, please consider buying me a coffee by clicking the link below. I'll do my best to keep this guide up to date and as user-friendly as possible. Thank you and take good care!

[![Donate](https://camo.githubusercontent.com/0283ea90498d8ea623c07906a5e07e9e6c2a5eaa6911d52033687c60cfa8d22f/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d50617950616c2d677265656e2e737667)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=R4QX73RWYB3ZA)