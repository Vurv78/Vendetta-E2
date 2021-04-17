## Vendetta ![Written In](https://img.shields.io/badge/Written%20In-Expression2-blue?style=flat-square) ![Size](https://img.shields.io/github/repo-size/Vurv78/Vendetta-E2?color=red&label=Size&style=flat-square) ![Sin](https://img.shields.io/github/license/Vurv78/Vendetta-E2?color=green&label=License&style=flat-square) [![github/Vurv78](https://discordapp.com/api/guilds/824727565948157963/widget.png)](https://discord.gg/epJFC6cNsw)


This is a Discord Bot Library I made after making discord-bot-rs and wanting to take a break from the cancer that is low-level programming

Here's this god forsaken e2 code that I've slapped a LICENSE.md on cause I felt like it

It is fully functional though, correctly sends heartbeats, makes sure discord acks back. It has everything you could use in the discord REST api in terms of bots, there is no capability for sending messages from the bot to the server though. This is because E2 doesn't allow POST requests or http headers at all (or at least until I add it in 500 years)

### Dependencies
[VExtensions](https://github.com/Vurv78/VExtensions) and [AEWebsocket Core Github](https://github.com/Andrew-Eathan/aewebsocketcore), which is a modified version of [Websocket Core](https://steamcommunity.com/sharedfiles/filedetails/?id=1773811033), they both do the same thing, but AEWebsocket core renames a few things.  
If you have Websocket Core and not AEWebsocket Core, then you'll have to manually rename the used functions in the library. Good thing it's less than 200 lines.

### Features
* Low level discord api usage, meaning you can do anything you want really and don't need an extension to this library
* Event system, run on certain discord api events, interact with given data
* Automated abstracted heartbeat, makes sure discord acknowledges heartbeats sent, closes the bot automatically when the chip is deleted.
* Support for multiple bots running at once
* Consistent function namesake with the vn* prefix
* ~500 ops maximum, ~100 cpu usage at any time
* Asynchronous HTTP using coroutines, see vnHTTPRequest
* Webhook message sending

### TODO
* Commands API (Just some abstraction to keep a prefix and easily make commands)
* Reconnecting when we disconnect for some reason (I've never encountered disconnects)
* Sharding (Never gonna happen dude)

### Example Code:

```c++
@name Discord Bot Relay
@persist Bot:table
@persist [BLACK PURPLE WHITE]:vector # Colors
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
    function on_ready(Ctx:table){
        local User = Ctx["user",table]
        print(format("Logged in as bot %s#%s",User["username",string],User["discriminator",string]))
        # You receive a ton of information about the bot here.
    }
    Bot:vnAddIntent(VN_INTENT["GUILD_MESSAGES",number]) # Subscribe to getting message events (MESSAGE_CREATE, etc)

    Bot:vnOnEvent("READY","on_ready")
    Bot:vnOnEvent("MESSAGE_CREATE","on_message") # When a message is created. (Can be made by a bot, system, user, webhook, so make sure you know which is which)
    Bot:vnConnect() # Start the bot
}
```
