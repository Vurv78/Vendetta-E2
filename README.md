## Vendetta-E2

This is a Discord Bot Library I made after making discord-bot-rs and wanting to take a break from the cancer that is low-level programming

Here's this god forsaken e2 code that I've slapped a LICENSE.md on cause I felt like it

It is fully functional though, correctly sends heartbeats, makes sure discord acks back. It has everything you could use in the discord REST api in terms of bots, there is no capability for sending messages from the bot to the server though. This is because E2 doesn't allow POST requests or http headers at all (or at least until I add it in 500 years)

Example Code:

```c++
@name Discord Bot Relay
@persist Bot:table

@persist [BLACK PURPLE WHITE]:vector # Colors
@persist LINK_CHANNEL_ID

#include "vendetta"
if( first() ){
    # Constants
    BLACK = vec(0)
    PURPLE = vec(151,117,209)
    WHITE = vec(255)

    Bot = discordBot("xxxxxxxx") # Put your bot token here. Server owners can see it so..

    function on_message(Ctx:table){
        # https://discord.com/developers/docs/resources/channel#message-object
        local Username = Ctx["author",table]["username",string]
        printGlobal(BLACK,"[",PURPLE,"Discord",BLACK,"] ",PURPLE, Username, WHITE,": ", Ctx["content",string])
    }
    function on_guild(Ctx:table){
        # Received guild info
        printTable(Ctx)
    }
    function on_ready(Ctx:table){
        local User = Ctx["user",table]
        print(format("Logged in as bot %s#%s",User["username",string],User["discriminator",string]))
        # You receive a ton of information about the bot here.
    }
    Bot:vnAddIntent(VN_INTENT["GUILD_MESSAGES",number]) # Subscribe to getting message events (MESSAGE_CREATE, etc)

    Bot:vnOnEvent("READY","on_ready")
    Bot:vnOnEvent("MESSAGE_CREATE","on_message")
    Bot:vnConnect()
}
```