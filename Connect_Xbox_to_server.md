# Connect to your server on Xbox

Xbox usually only presents the featured servers, and no option to manually add your own. However, there is a workaround for this.

## Change the Xbox DNS settings

- Go to your network settings on the xbox, and advanced settings. Select DNS, and change from automatic to manual. 
- Set the primary DNS as: `104.238.130.180`, and the secondary as: `1.1.1.1`.
  - FYI this is the DNS redirection for [BedrockConnect](https://github.com/Pugmatt/BedrockConnect), it only redirects the featured servers.

- Quit Minecraft if it was open, restart the console
- Note: if you use Pi-Hole you may have to temporarily disable it

## Alternative method, using Pi-Hole

If you have a Pi-Hole, go to the admin console, Local DNS, and add the following entries:

| Domain                  | IP              |
| ----------------------- | --------------- |
| geo.hivebedrock.network | 104.238.130.180 |
| hivebedrock.network     | 104.238.130.180 |
| mco.mineplex.com        | 104.238.130.180 |
| play.inpvp.net          | 104.238.130.180 |
| mco.lbsg.net            | 104.238.130.180 |
| play.galaxite.net       | 104.238.130.180 |
| play.pixelparadise.gg   | 104.238.130.180 |

## Launch a server, and add your own

- Launch Minecraft, play, servers. Pick any featured server and hit play. 
- Now, rather than automatically connecting to the featured server, a new menu should pop up that will allow you to add/select your own.
- Add your own server using the Public IPv4 address from your virtual machine above. Use the Port: `19132`. 
- Make sure to toggle on the option to save this sever in your list, and give it a cool name.
- Connect and enjoy!
- To connect again later, once again select any featured server and hit play, now your server will show up in the new add/select server menu. 
- Note: This menu appears to persist after re-enabling Pi-Hole; however, I've kept the manual DNS settings on the Xbox for now.

