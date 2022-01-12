<div align="center">
	
**[View on DevForum](https://devforum.roblox.com/t/solar-admin-system-lightweight-command-bar/1624370)**

  
# Solar Admin System :sparkles:
**Beta Release v0.1**

[Download Model :hammer_and_wrench:](https://github.com/Starnamics/Solar/releases/latest)
 [Get Model on Roblox :inbox_tray:](https://www.roblox.com/library/8505743656/Solar-Admin-System)
<hr>

## Notice:
Solar Admin is still in development and may/will have many bugs.
It is not recommended you use this in production games while it is in beta.

There are many features missing at the moment which will be implemented at a later date

<hr>

# About Solar
Solar is an admin system designed primarily for developers.
It allows developers to easily customize almost all aspects of it.
Developers are encouraged to program their own commands to add to Solar.

Solar comes with an easy to use API which allows developers to expand Solar even more.

Solar does **not** come with any commands besides the `example`  command, however, you can download some pre-made commands on our Discord Server or at the bottom of this post
<br>
<hr>

</div>

# Documentation
# Get Started

Setting up Solar is fairly easy, follow the steps below to get it setup in your game.

## Downloading & Initializing Solar
You can download Solar two ways, [**GitHub**](https://github.com/Starnamics/Solar/releases/latest) & [**Roblox**](https://www.roblox.com/library/8505743656/Solar-Admin-System)

Once you have Solar downloaded, insert it into your game and then drag it into `ReplicatedStorage`

Next you'll need to initialize Solar to begin using it. To do this, insert a new `Script` into `ServerScriptService` and paste the following code into it:
```lua
local Solar = game:GetService("ReplicatedStorage"):WaitForChild("Solar")
local API = require(Solar:WaitForChild("API")) --// Require Solar API

--// Initialize Solar & register commands
API:Initialize()

--// If you do not want to register commands by default then set the 1st argument to false
--// API:Initialize(false)
```

Solar will not work unless you initialize it

## Configuring Solar
Once you have initialized Solar, you'll need to configure it in the Settings module in order to start using it!

To get started, open the `Settings` module in the Solar folder

Once you have it open, scroll down to the `Settings` table in the module and configure the values however you wish

Everything is documented within the module so you should be able to figure out how to do everything

# Adding Commands
Adding Commands to solar is pretty easy, first you'll need to find a command (or command pack) you'd like to add and then insert it into your commands folder (by default it is located under the `Configuration` folder

Once you've dragged your command into the commands folder, it should be usable in game once it has been registered

It is recommended you remove the example command unless it is needed for testing

# Configuring Commands
Commands are usually configured by default but if you'd like to change the configuration for a command (such as the permission level or name) you can open the command module and edit the values

# Creating Commands
If you'd like to create your own commands rather than using premade commands, you can do so with ease!

Below is a template for a command:

```lua
local API = require(game:GetService("ReplicatedStorage"):WaitForChild("Solar"):WaitForChild("API"))

local command = {
	Name = "example", --// The name of your command
	Description = "Just an example command!", --// Short description of what your command does
	Arguments = { --// Command arguments
		[1] = {
			Name = "text", --// The name of the argument
			Required = true, --// Is this argument required?
			Type = "string", --// The argument type
		},
		
		[2] = {
			Name = "target", --// The name of the argument 
			Required = true, --// Is this argument required?
			Type = "player", --// The argument type
		},
		
		[3] = { 
			Name = "reason", --// The name of the argument
			Required = false, --// Is this argument required?
			Type = "multistring", --// The argument type
		},
	},
	PermissionLevel = 3, --// The permission level required to run this command
	ParseArguments = true, --// Should the arguments be parsed automatically or should they be sent as strings
}

function command.execute(player,arguments) --// This function is called when someone executes the command
	print("Hello from this command! The arguments entered were:",arguments)
	return true	 --// Make sure to return something if the command was successfully executed!
end


--// If command is required on the client, do not return the execute function
if game:GetService("RunService"):IsClient() then
	command["execute"] = nil
end

return command
```

You can edit the name of the command by changing the `Name` value (you must change the name of your command module to the same value)

You can also add a description to help users understand what the command does.

## Arguments

Commands also have arguments which are pretty easy to be configured and Solar will do the heavy lifting for you (parsing the arguments, etc)

Here is a list of valid argument types:
- player - Player
- string - Single String
- multistring - Multiple Strings
- number - Number
- boolean - True or False

The `player` argument will be parsed as a `RegisteredPlayer` instead of a `Player` instance. You can access the `Player` instance via `RegisteredPlayer.Player`

The `Required` value for arguments will force the command to fail to execute if the required argument is missing.

The `Name` value is the name of the argument.

## Permissions
Command Permissions are useful for restricting commands to certain groups of players.

Solar has 4 permission levels,
- **1** - Player
- **2** - Moderator
- **3** - Administrator
- **4** - Head Administrator

You should set the command permission to the correct permission level in order to prevent abuse of it. (for example, server changing commands should be restricted to administrators)

## Execution
The command module also contains an `execute` function, this is the main function of your command and is fired whenever a command is executed.

When fired, two values are sent:
- **player** - the player who executed the command
- **arguments** - the command arguments

You can program your command in this function and make use of those values.

You must return a value other than `nil` if the command is executed successfully 

# Solar API
Solar comes with an API that commands & plugins can make use of.

## Functions
```lua
function API:Initialize(RegisterCommandsByDefault: boolean?,RegisterCurrentPlayers: boolean?) : void

RegisterCommandsByDefault:
   Should Commands in the Commands folder be registered by default | default: true
RegisterCurrentPlayers:
   Should current players be registered | default: true
````

```lua
function API:RegisterPlayer(player: Player) : RegisteredPlayer

player:
   Player Instance
```

```lua
function API:UnregisterPlayer(player: Player) : boolean

player:
   Player Instance
```

```lua
function API:GetPlayers() : {RegisteredPlayer}
```

```lua
function API:GetPlayer(player: Player | string) : RegisteredPlayer

player:
   Player Instance or Player Name
```

```lua
function API:AddEventListener(name: string,folder: Folder) : BindableEvent

name:
   Name of the Event Listener
folder:
   Folder of the Event Listener
```

```lua
function API:AddRemoteEvent(name: string,folder: Folder) : RemoteEvent

name:
   Name of the Remote Event
folder:
   Folder of the Remote Event
```

```lua
function API:RegisterCommands(commands: {ModuleScript}) : Commands

commands:
   Table of Command module scripts
```

```lua
function API:ExecuteCommand(player: Player,command: string,arguments: {}?) : boolean | any

player:
   Player Instance
command:
   Name of command
arguments:
   Dictionary of arguments
```

```lua
function API:GetCommandInstance(command: string) : ModuleScript

command:
   Name of command
```

```lua
function API:GetCommands() : {ModuleScript}
```

## Classes
```lua
RegisteredPlayer:

string Username: Player Username
string DisplayName: Player Display Name
number UserId: Player UserId
number AccountAge: Player Account Age
number PermissionLevel: Player Permission Level
number JoinTime: Time player joined
player Player: Player Instance
{any} PolicyInfo: Player Policy Info
string Region: Player Region
{any} ExecutionLog: Player execution log
{any} GameHistory: Player history

function RegisteredPlayer:LogHistory(history: {any}?) : {any}
   history: Table to add to player history

function RegisteredPlayer:LogExecution(command: string,arguments: {}?,response: any) : {any}
   command: Name of command
   arguments: Command arguments
   response: Execution response

function RegisteredPlayer:SetPermission(permissionLevel: number | nil) : RegisteredPlayer
   permissionLevel: Permission Level or nil for default permission
```

# Roadmap
Here are the current planned features for Solar
- Custom Plugins
- Custom Themes
- Built-in UI Library

# Commands & Command Packs

**Command Packs:**
- [Moderation Pack](https://www.roblox.com/library/8517379100/Moderation-Pack-Solar)
-- Kick
-- Ban
-- *more coming soon*

**Commands:**
Currently there are no individual commands

**More commands & packs will be releasing soon!**

# Thank you!
Thank you for reading this post. 

If you find any bugs with Solar, please let me know in the replies :bug:

If you have any feedback regarding Solar, you can also tell me it in the replies :)
