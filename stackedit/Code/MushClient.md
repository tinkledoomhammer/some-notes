# MushClient scripting, plugins, etc
[http://www.gammon.com.au/scripting]

**Useful references**

-   [MUSHclient scripting functions](http://www.gammon.com.au/mushclient/functions.htm)
-   [MUSHclient online documentation](http://www.gammon.com.au/scripts/doc.php)
-   [Lua scripting forum](http://www.gammon.com.au/forum/bbshowpost.php?bbtopic_id=113)
-   [Script callbacks for MXP](http://www.gammon.com.au/scripts/doc.php?general=mxp_callbacks)
-   [Script callbacks for plugins](http://www.gammon.com.au/scripts/doc.php?general=plugin_callbacks)
-   [List of script return codes](http://www.gammon.com.au/scripts/doc.php?general=errors)
-   [Functions help list](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=6120)

**Lua-specific items**

-   [Lua script extensions](http://www.gammon.com.au/scripts/doc.php?general=lua)
-   [Lua tables](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=4903)
-   [Lua tables in detail](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=6036)
-   [Converting Lua tables to MUSHclient variables](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=4960)
-   [Accessing MUSHclient variables by using a Lua pseudo-table](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=4904)
-   [Using the string library: find, gfind and gsub](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=6034)
-   [Extending Lua with your own DLLs](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=4915)
-   [Building pauses into scripts by using coroutines](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=4956)
-   [Pausing a script until certain output arrives from the MUD](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=4957)
-   [Using Perl-Compatible Regular Expressions (PCRE) in Lua](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=4905)
-   [Doing AES encryption](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=4988)
-   [Compression and decompression](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=4989)
-   [Hashing, base64 encoding and decoding](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=4990)
-   [Accessing COM objects from Lua](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=6022)
-   [Accessing MySQL database from Lua](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=5983)
-   [Bit manipulation](http://www.gammon.com.au/forum/bbshowpost.php?bbsubject_id=4907)

Also, our own versions of the help on Lua  [base](http://www.gammon.com.au/scripts/doc.php?general=lua_base),  [coroutines](http://www.gammon.com.au/scripts/doc.php?general=lua_coroutines),  [debug](http://www.gammon.com.au/scripts/doc.php?general=lua_debug),  [io](http://www.gammon.com.au/scripts/doc.php?general=lua_io),  [math](http://www.gammon.com.au/scripts/doc.php?general=lua_math),  [os](http://www.gammon.com.au/scripts/doc.php?general=lua_os),  [package](http://www.gammon.com.au/scripts/doc.php?general=lua_package),  [string](http://www.gammon.com.au/scripts/doc.php?general=lua_string), and  [table](http://www.gammon.com.au/scripts/doc.php?general=lua_tables)  functions.

**Documentation from other sites**

-   [Programming in Lua](http://www.lua.org/pil/)
-   [Lua documentation](http://www.lua.org/docs.html)
-   [VBscript and Jscript help files](http://download.microsoft.com/download/winscript56/Install/5.6/W982KMeXP/EN-US/scrdoc56en.exe)

**Script engine downloads from other sites**

-   [Perlscript download site](http://www.activestate.com/ActivePerl/download.htm)
-   [Python script download site](http://www.activestate.com/ActivePython/download.htm)

**Tutorial videos**

-   [Video showing how to make a trigger](http://vimeo.com/80327806)
-   [Video showing how to make a targetting alias](http://vimeo.com/80326272)
-   [Video showing how to make a plugin](http://vimeo.com/80326690)


## Script config


### world.SetTriggerOption
**Script function**
 [Read about scripting](https://www.gammon.com.au/scripts/doc.php?general=scripting)  

**Type** Method
**Summary** Sets the value of a named trigger option
**Prototype**
`long SetTriggerOption(BSTR TriggerName, BSTR OptionName, BSTR Value);`
 [View list of data type meanings](https://www.gammon.com.au/scripts/doc.php?general=data_types)  

#### Description
Sets the current value of a trigger option.  
  
You must specify the name of an existing trigger, and a trigger option from the list given for GetTriggerOption. These are the same names as used in the XML world files for trigger options.  
  
Note that you can neither get or set the "name" field. It is listed below for completeness in viewing triggers copied to the clipboard or in world files and plugins. You already know the name when getting the trigger option (it is TriggerName), and this function is provided to modify existing triggers, not rename them. To change the name of a trigger use ExportXML, modify the XML, and then ImportXML.  
  
If SetTriggerOption is called from within a plugin, the triggers for the current plugin are used, not the "global" MUSHclient triggers.  
  
For numeric options (such as "sequence") the supplied string will be converted to a number. It is an error if the string does not consist of digits (0 to 9). For the "user" option an optional leading sign is accepted.  
  
For boolean options (such as "enabled") the supplied string may be:  
  
"y", "Y", or "1" for true  
"n", "N", or "0" for false  
  
Also if you are using Lua you can supply simply true or false.  
  
#### Option Names  
  
"clipboard_arg": 0 - 10 - which wildcard to copy to the clipboard  
"colour_change_type": 0=both, 1=foreground, 2=background  
"custom_colour": 0=no change, 1 - 16, 17=other  
"enabled": y/n - trigger is enabled  
"expand_variables": y/n - expand variables (like @target)  
"group": (string - group name)  
"ignore_case": y/n - caseless matching  
"inverse": y/n - only match if inverse, however see match_inverse  
"italic": y/n - only match if italic, however see match_italic  
"keep_evaluating": y/n - evaluate next trigger in sequence  
"lines_to_match": 0 - 200 - how many lines to match for multi-line triggers  
"lowercase_wildcard": y/n - make matching wildcards lower-case  
"match": (string - what to match)  
"match_style": (see below) - what style and colour combination to match on  
"multi_line": y/n - multi-line trigger  
"name": (string - name/label of trigger)  
"new_style": (style(s) to change to: (bold (1) / underline (2) / italic (4) ) )  
"omit_from_log": y/n - omit matching line from log file  
"omit_from_output": y/n - omit matching line from output window  
"one_shot": y/n - trigger is deleted after firing  
"other_back_colour": (string - name of colour to change to)  
"other_text_colour": (string - name of colour to change to)  
"regexp": y/n - regular expression  
"repeat": y/n - repeatedly evaluate on same line  
"script": (string - name of function to call)  
"send": (multi-line string - what to send)  
"send_to": 0 - 14 - "send to" location (see below)  
"sequence": 0 - 10000 - sequence in which to check - lower first  
"sound": (string - sound file name to play)  
"sound_if_inactive": y/n - play sound even if world inactive  
"user": -2147483647 to 2147483647 - user-defined number  
"variable": (string - name of variable to send to)  
  
  
Options that match on colour, bold, inverse or underline are tested against the first matching character.  
  
  
#### Send-to locations  
  
"0" - send to MUD  
"1" - put in command window  
"2" - display in output window  
"3" - put in status line  
"4" - new notepad  
"5" - append to notepad  
"6" - put in log file  
"7" - replace notepad  
"8" - queue it  
"9" - set a variable  
"10" - re-parse as command  
"11" - send to MUD as speedwalk  
"12" - send to script engine  
"13" - send without queuing  
"14" - send to script engine - after omitting from output  
  
  
#### Trigger match_style field 
will be as follows (in hex nibbles):  
  
TBFS  
  
T = what style to test  
B = background colour  
F = foreground colour  
S = what style bits are wanted  
  
Style bits:  
  
NORMAL 0x0000 // normal text  
HILITE 0x0001 // bold  
UNDERLINE 0x0002 // underline  
BLINK 0x0004 // italic (sent to client as blink)  
INVERSE 0x0008 // need to invert it  
  
  
MATCH_TEXT 0x0080 // match on text colour?  
MATCH_BACK 0x0800 // match on background colour?  
MATCH_HILITE 0x1000 // match on bold bit?  
MATCH_UNDERLINE 0x2000 // match on underline?  
MATCH_BLINK 0x4000 // match on blink?  
MATCH_INVERSE 0x8000 // match on inverse?  
  
Colours:  
  
BLACK = 0  
RED = 1  
GREEN = 2  
YELLOW = 3  
BLUE = 4  
MAGENTA = 5  
CYAN = 6  
WHITE = 7  
  
  
See the forum posting at http://www.gammon.com.au/forum/?id=4873 for more details about match_style.  
  
An easier way of settting trigger options, particularly ones relating to matching style and colours, could be to use ImportXML.  
  
See the forum posting at http://www.gammon.com.au/forum/?id=7123 for more details about ImportXML, and the "addxml" module supplied for Lua scripting.`



## Aardwolf formats

### invdata
```
----------------------------------------------------------------------------
Help Keywords : Invdata Eqdata.
Help Category : Equipment.
Related Helps : Invdetails, Invmon, ObjectId, Invsort.
Last Updated  : 2018-01-10 21:14:10.
----------------------------------------------------------------------------

Syntax: invdata (<container id>)
        eqdata
        invdata / eqdata ansi

The 'invdata' and 'eqdata' commands are designed for use with client-side
scripts and plugins.  They provide a standardized display of information
about your own worn equipment (with 'eqdata') or carried inventory
('invdata').  You may also use a container's objectid as an argument for
invdata.  Doing so will list the output for the given container instead of
carried inventory.

The 'ansi' option causes the output to include regular ANSI color rather 
than the Aardwolf color codes.

See 'help objectid' for a description of what object ID numbers mean and
their intended use.

The output for these commands is as follows:

{tag}
objectid,flags,itemname,level,type,unique,wear-loc,timer
...
{endtag}

Tag: Displays eqdata, invdata, or invdata container-objectid, based on
which argument was used.

Flags: Lists any of the following flags as a string of letters (eg
KIG); if none, field is blank.

      K : Kept        I : Invis
      M : Magical     G : Glowing
      H : Humming     T : Temporary affect on item              

Itemname: Gives the item name, with MUD color codes.

Level: Item's level.

Type: Item type, from the following list.

      1 : Light              15 : Boat
      2 : Scroll             16 : Mob Corpse
      3 : Wand               17 : Player Corpse
      4 : Stave              18 : Fountain
      5 : Weapon             19 : Pill
      6 : Treasure           20 : Portal
      7 : Armor              21 : Beacon
      8 : Potion             22 : Gift Card
      9 : Furniture          23 : Unused
     10 : Trash              24 : Raw Material
     11 : Container          25 : Campfire
     12 : Drink Container    26 : Forge
     13 : Key                27 : Runestone
     14 : Food

Unique: Returns 1 if unique (see 'help object flags'), 0 if not.

Wear-loc: Number of current wear-location.  This has the potential to 
vary from race to race; type 'wearable' for a list of location numbers.  
Returns -1 if the item is not worn.

Timer: Number of seconds until item disintegrates, or -1 if item has no
timer.

Endtag: After all items have been listed, displays either /eqdata or
/invdata, depending on the command used.  Container ID is not displayed.
----------------------------------------------------------------------------
```




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM1NTg5MjkyMl19
-->