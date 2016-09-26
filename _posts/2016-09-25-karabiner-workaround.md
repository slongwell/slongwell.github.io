---
layout: post
title:  "Karabiner is Karabroken on macOS Sierra. Here's a Temporary Solution."
date:   2015-09-25 22:36:00
comments: true
---
I've used [Karabiner][kb] for Mac to embed my arrow keys in a Diamond-style configuration - in other words, `j-l-i-k` maps to `left-right-up-down`. This configuration is active when the caps lock key is held. (*Note*: the [Seil][seil] utility is specifically required for remapping `CAPS`. I remapped `CAPS` to `CTRL_R`, which is not present on my MBP. Consequently, `CTRL_R` is technically the modifier key). I've similarly embedded `BKSP` and `DEL` as the `o` and `p` keys and added additional 'super-modifier' keys: `f` to signify 'word at a time' (e.g. `CAPS-f-j` jumps back a word), `d` to go to beginning/end of line, and `s` to highlight a selection. Karabiner can also detect the vendor ID associated with a connected keyboard, so it can apply specific remappings to specific keyboards (e.g. the command and option keys can be switched for a PC keyboard while leaving the bindings for the built-in MBP keyboard unaltered). Finally, I've been experimenting with using `CMD_R` as a key for exposing all windows and as a toggle key for switching between spaces using `j` and `l`. If you're curious about implementing any of these features, see my `private.xml` file [here][private].

<!--more-->

I recently updated to macOS Sierra from El Cap, which to my dismay caused Karbiner and Seil to stop working. [The author][author] (bless him) is hard at work on a new version called [Karabiner-Elements][kbe]. However, the early release (v0.90.40) does not have a usable GUI interface and only supports basic behavior like switching keys. 

Fortunately, macOS features [built-in support][cocoa] for altering keybindings by creating a [property list][pl] (.dict file) in the user library directory. This property list can be written in either XML or the 'old-ASCII/NeXT' style (the example below uses the old-style). A complete listing of possible actions is available in the [developer documentation][ref] for the macOS API.

***

My workaround uses Karabiner-Elements to do the heavy lifting to remap `CAPS` to `CTRL_R`. When either `CTRL` key is held, a property list specifies the arrow keys as `jlik`, `BKSP` as `o`, and `DEL` as `;`.  

Disclaimer: [I've][noidea] never taken a class in operating systems.

1. Install [Karabiner-Elements][kbeu]
2. In Karabiner-Elements, remap `CAPS` to `CTRL_R`:

    *In terminal*: 
    
        mkdir -p ~/.karabiner.d/configuration/
	    cd ~/.karabiner.d/configuration/  
	    touch karabiner.json  
	    open karabiner.json  

	*Into the text editor that opens, paste and save*: 

	    {
	        "profiles": [
	            {
	                "name": "Default profile",
	                "selected": true,
	                "simple_modifications": {
	                    "caps_lock": "right_control"
	                }
	            }
	        ]
	    }

3. In the user library, create a keybinding property list:  

    *In terminal*:

	    mkdir -p ~/Library/KeyBindings
	    cd ~/Library/KeyBindings
	    touch DefaultKeyBinding.dict
	    open DefaultKeyBinding.dict

	*Into the text editor that opens, paste and save*:  
	
        {  
	        /* Diamond arrows */  
	        "^j" = "moveBackward:";  
	        "^l" = "moveForward:";  
	        "^i" = "moveUp:";  
	        "^k" = "moveDown:";  
      
	        /* Backspace; Delete */  
	        "^o" = "deleteBackward:";  
	        "^;" = "deleteForward:";  
        }



*Note*: Before these changes take effect in a given application, the application must be restarted. A full computer restart is ideal.

The resulting mapping is far less powerful - it lacks 'super-modifier' keys, keyboard vendor ID detection, and the ability to discern left and right `CTRL/OPT` keys. However, it should be an effective stopgap until Karabiner-Elements reaches maturity. Likewise, I'm only scratching the surface - I'm sure these missing features are possible (and probably easy) to implement, and, without delving into source code, I'd suspect Karabiner itself interacts with macOS in a similar manner, albeit with a C-based API.

See also:   
[Customizing the Cocoa Text System][a]  
[How to Create Keyboard Layout and Keybinding][b]  
[KeyBindingsEditor][c]  


[kb]: https://pqrs.org/osx/karabiner/
[seil]: https://pqrs.org/osx/karabiner/seil.html.en
[private]: /files/private.xml
[kbe]: https://github.com/tekezo/Karabiner-Elements
[author]: https://pqrs.org/profile.html.en
[kbeu]: https://github.com/tekezo/Karabiner-Elements/tree/master/usage

[cocoa]: https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/EventOverview/TextDefaultsBindings/TextDefaultsBindings.html#//apple_ref/doc/uid/20000468-CJBDEADF 
[pl]: https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PropertyLists/Introduction/Introduction.html
[ref]: https://developer.apple.com/reference/appkit/nsresponder

[noidea]: https://cdn-images-1.medium.com/max/600/1*snTXFElFuQLSFDnvZKJ6IA.png


[a]: http://www.hcs.harvard.edu/~jrus/Site/Cocoa%20Text%20System.html
[b]: http://xahlee.info/kbd/osx_keybinding.html
[c]: hhttp://www.cocoabits.com/KeyBindingsEditor/

