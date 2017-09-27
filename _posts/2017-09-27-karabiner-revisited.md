---
layout: post
title:  "Karabiner is No Longer Karabroken on macOS Sierra. Here's what I normally do."
date:   2017-09-27 11:00:00
comments: true
published: true
---
One year ago, I [posted][oldpost] how updating to macOS Sierra broke the existing implementation of Karabiner, disabling my beloved diamond-arrows keyboard layout. I detailed a janky initial fix (which failed to work within many programs, like PyCharm). Later, in the comments I included a download to a tweaked [.dmg][dmg] of Karabiner-Elements (K-E) that fixed those issues. However, I still experienced occassional hiccups with this solution, like stuck <kbd>CMD</kbd> key behavior, or worse, stuck keys at the login password prompt. I never got around to explaining the changes I made - mostly because I was hoping that K-E would have complete functionality soon.

That day has come.

<!--more-->
In the old Karabiner, there was a [`private.xml`][private] that could be used to define intricate key remappings. Now that K-E has its first stable release (v11.0.0), this functionality has returned in the form of [Complex Modifications][comp-mods], which are defined with `.json` config files instead of a `.xml` file. These files are stored in `~/.config/karabiner/assets/complex_modifications`.

To implement my favorite key mappings:

1. Install the [latest version of K-E][ke_download]
2. In `K-E > Simple Modifications`:
* remap <kbd>CAPS</kbd> to <kbd>CTRL_R</kbd>
* remap <kbd>CMD_R</kbd> to <kbd>Mission Control</kbd>
3. Download my [`.json` config file][json]
4. In `K-E > Complex Modifications`:
* Enable `ctrl_r + IJKL to arrows; ctrl_r + o/semi to backspace/delete`

This will implement the following behavior:

General remappings:
* <kbd>CAPS</kbd> maps to <kbd>CTRL_R</kbd> 
* <kbd>CMD_R</kbd> maps to <kbd>Mission Control</kbd>

When <kbd>CTRL_R</kbd> (i.e. <kbd>CAPS</kbd>) is held:
* <kbd>f</kbd> maps to <kbd>OPT_R</kbd> when held (i.e. as a 'super-modifier')
* <kbd>j-l-i-k</kbd> maps to <kbd>left-right-up-down</kbd> ('diamond-arrows')
* <kbd>o</kbd> maps to <kbd>BKSP</kbd>
* <kbd>;</kbd> maps to <kbd>DEL</kbd>

My MBP lacks a <kbd>CTRL_R</kbd> key, which is why I use it as an intermediate modifier for <kbd>CAPS</kbd> (I've struggled to get the remappings to work directly with <kbd>CAPS</kbd>). Holding <kbd>f</kbd> (<kbd>OPT</kbd>) in addition to <kbd>CAPS</kbd> has the effect of making arrows and deletes occur 'word-at-a-time', permitting faster navigation and editing (I term this an <kbd>OPT</kbd> 'super-modifier'). I find the <kbd>CMD_R</kbd> mapping to Mission Control is a great way to quickly switch windows and desktops. 

***
So far, this implementation has been bug-free for me and has the added benefit of compatibility with future K-E updates. In the future, I hope to add <kbd>s</kbd> as a <kbd>SHIFT</kbd> 'super-modifier', allowing easier highlighting with the diamond-arrow keys. Thanks again to the [author][author] of K-E for all his hardwork!

[oldpost]: http://slongwell.github.io/articles/2016-09/karabiner-workaround
[dmg]: http://disq.us/p/1eflhzc

[comp-mods]: https://github.com/pqrs-org/KE-complex_modifications
[ke_download]: https://pqrs.org/osx/karabiner/index.html
[json]: karabiner://karabiner/assets/complex_modifications/import?url=/files/sal-diamond.json

[private]: /files/private.xml
[author]: https://pqrs.org/profile.html.en
