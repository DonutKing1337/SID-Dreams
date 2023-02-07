================================================
Mode 7 Plugin for RPG Maker 2003 ver 1.00
For DynRPG version 0.13 or higher
By Kazesui
================================================

This Plugin allows you to render maps in a style similar to that of the mode 7 protocol on the snes console.
In plaintext: it will allow you to add depth to one flat texture, here given by the graphics of the map.

WARNING
-------------

There are several issues with the current implementation. Make sure to check out the "known problems" section 
before asking about it in a forum post.


Installation
-------------

To install the plugin, make sure that you have patched your project with cherry's DynRPG patch
which can be found here: http://cherrytree.at/dynrpg

Then go to the DynPlugins folder in this demonstration project and copy both the "DynModeSeven.dll" to the DynPlugins folder in your own project. You would possibly want to
Copy the content of the "DynRPG.ini" file in the demonstration project folder as well.


Instructions
-------------

To render a map in mode 7 style you must first call the "load_map_to_texture" command, followed by "init_mode7" command.
You will then have to call the "show picture" command with an ID corresponding to your mode7_id. 
By default, this is set to 1, but can be configured using the "@set_mode7_picture_id" command. 
Any picture's of the same or lower id as the selcted picture id, will be obscured by the mode 7 rendered map, so choose carefully.

Most of the properties of the rendering can be set by comment commands. These are commands invoked by writing them using the event
comments in the event command menu in rm2k3. The comment commands can look like this:

@add_sprite "castle", "Picture/castle4.png", 64, 64, 1, 1, 15, V1, V2

"@add_sprite" is the name of the command, and the name indicates what happens when called. All that follows the command name are
parameters and separated by commas. These specifies how the command should be executed. 
Any text given as a parameter should be written in quotation marks. V1 is a special type of parameter, which will submit the value
stored in Variable 0001 instead of a static number. V2, would indicate variable 0002 being used instead and so on. VV2 can be used
as a pointer, where the value would be retrieved from wherever Variable 0002 is pointing at.

IMPORTANT
Any graphics used during mode7 rendering, must already be present in the project folder. This means that any charset or tilesets
being used, must already have been imported. Using RTP which has not been imported will return in errors!!


Commands
-------------

@load_map_to_texture
(none)	 no parameters
       / This command reads information on the map from the map file, and stores them for later use.
	 Only one map will be stored in the plugin, so if you wish to render a different map in mode7 style
	 you will have to call this command again.

@init_mode7
(text)	 parameter#1: yes or true to activate, no or false to deactivate.
       / This command activates mode 7 rendering if active. If you wish to use the picture id 
	 associated with mode 7 rendering, you will have to deactive this, as it will otherwise 
	 be obscured by the mode7 rendering.

@is_mode7_rendering
(number) parameter#1: switch id
       / This commands sets switch of chosen id to on or off depending on whether init_mode7 	 has been activated or not. When you save a game, the plugin doesn't save whether 
	 the map is rendered in mode7 or not, nor any configuration you have mode. By calling 
	 this command, you will be able to tell if mode7 is active or not after loading from a 
	 file, and can reconfigure the map to render it in proper mode 7 again.

@set_mode7_picture_id
(number) parameter#1: picture id
       / This commands let you choose which picture id you want the mode7 rendering to be
	 associated with. by default this is set to picture id 1.

@rotate_to
(number) parameter#1: angle
       / Rotates the texture to a specific angle in degrees. default is 270

@rotate_right_by
(number) parameter#1: angle increment
       / Rotates the texture to the right by a given increment

@rotate_left_by
(number) parameter#1: angle increment
       / Rotates the texture to the left by a given increment

@add_sprite
(text)	 parameter#1: tag
(text)	 parameter#2: filename
(number) parameter#3: width
(number) parameter#4: height
(number) parameter#5: angles
(number) parameter#6: animation frames
(number) parameter#7: event id
(number) parameter#8: x pixel offset (optional)
(number) parameter#9: y pixel offset (optional)
       / Sets the graphic of an event one from a custom spritesheet. Look up "Spritesheet" 
	 to see how to format your custom spritesheets. Notice that animation frames has 
	 not been implemented yet and must always be set to 1.

@change_hero_sprite
(text)	 parameter#1: filename
(number) parameter#2: width
(number) parameter#3: height
(number) parameter#4: angles
       / Changes the sprite of the hero into one given from a custom spritesheet. Look up 	 spritesheet to see how to format your custom spritesheets.

@set_background
(text)	 parameter#1: filename
       / Selects a background to be shown at the horizon. The background will move relative 	 to rotation and the movement speed is dictated by the width of the background. 
	 The background should be at least 720 pixels wide, preferably wider and divisiable 
	 by 16.

@using_background
(text)	 parameter#1: yes or true to activate, no or false to deactivate
       / Used to enable or disable the use of backgrounds. If disabled, the mode7 rendering 	 will skip drawing over where the background should be, meanig that you will be 
	 able to see graphical objects from beneath the mode7 map, including the original map. 
	 This options allows you to use lower picture ids to create your own background, with 
	 the greater customisability which comes with it. Enabled by default.

@set_wrapping
(text)	 parameter#1: yes or true to activate, no or false to deactivate
       / Enables or disables wrapping of the map. Must be called if you have a world map 	 which should loop around once you reach the edge of the map. Disabled by default.

@set_boundary_texture
(number) parameter#1: X coordinate
(number) parameter#2: Y coordinate
       / Replicates a selected tile at the boundaries. Doesn't do anything if wrapping is 	 enabled.

@set_texture_scale
(number) parameter#1: Scaling factor
       / Sets the scaling factor of the mode 7 texture. Default is 260

@set_z_value
(number) parameter#1: Z value
       / Sets how height above mode 7 texture. Default is 90

@set_horizon
(number) parameter#1: scanline horizon
(number) parameter#2: numeric horizon
       / scanline horizon determines at what pixel line the texture will start being 	 rendered. The numeric horizon determines at what line of pixels will be used 
	 for perspective computations.
	 scanline horizon default: 50
	 numeric  horizon default: 25

@set_rotational_offset
(number) parameter#1: offset
       / Sets the offset of the point of rotation into the map. Default is 100

@unload_mode_seven
(none)	 no parameters
       / Will unload all data related to mode7 rendering. The plugin does this automatically
	 for "teleport" and "recall position" commands, but for other scenarios, you might 
	 need to manually unload mode seven rendering yourself to avoid errors.

Spritesheet
-------------

It's possible to use some custom spritesheets with this plugin. The format of these are given with
different angles of the sprite in question being listed horizontally. The first sprite should be
facing upwards, and the subsequent sprites should follow clockwise rotation.
In the commands related to adding custom sprites, width specifies the width of a single sprite,
and angles determine how many sprites are given horizontally. As such, the width of the entire image
must be at least angles * width.
Vertical alignment is reserved for animation frames, but has not yet been implemented.


Known problems
---------------

When selecting a tileset in map properties, the first tilset will NOT work with this plugin.
Some obscure internal memory problems prevent this, and as such, this is not likely to change
anytime soon.
##############

When wrapping is enabled, sprites will ocassionaly disappear before having gone out of the screen.
this is related to the rather dubious way of how rm2k3 handles screen relative coordinates in a wrapped map. This _might_ be fixed some time in the future. Also, wrapping enabled means that it wraps both ways of the map. Wrapping your map only one way while enabling wrapping, could lead to undefined behaviour and crashing the game.
##############

Animation tiles don't animate, as it simply has not been implemented yet.
It's will possibly be added in a future version of the plugin though.
##############

The map doesn't fade when switching maps or opening the menu.
For this, I suggest using a picture to manually cause the fading. You should be able to
do this for the menu as well, by disabling it on mode 7 maps, and then have a parallel
process check if the player is hitting the menu key, fade with picture and then manually
open the menu.
##############

Transparency of events does not work during mode 7 rendering. If you wish to make the hero
entirely transparent, you should simply create a charset with an empty char, and use this
as the hero sprite
##############

If further problems are found, you can send me a message on www.rpgmaker.net 
or www.multimediaxis.de/forums/5-RPG-Atelier. My nickname is Kazesui.