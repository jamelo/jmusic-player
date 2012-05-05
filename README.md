JMusic Player
=============

Copyright (c) 2012 Jordan Melo.
Licensed under GNU General Public License

Contact: jmelo@uwaterloo.ca

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see 
&lt;[http://www.gnu.org/licenses/](http://www.gnu.org/licenses/)>.

---------------------------------------------------------------------------


Functionality:
------------------------------------

- Standard playback controls and scrubbable playback head
- Built in song queue
- Load a playlist defined using XML
- Enqueue songs or playlists in the player via Javascript without
  interrupting playback.


Creating a playlist:
------------------------------------

All playlists are wrapped in &lt;playlist> and &lt;/playlist>. Each song is defined
by putting the path to the file in between &lt;song> and &lt;/song> tags. That's it!

Here's an example playlist.xml file:

<pre>
&lt;?xml version="1.0" encoding="UTF-8"?>
&lt;playlist>
    &lt;song>music/song1.mp3</song>
    &lt;song>music/song2.mp3</song>
    &lt;song>music/song3.mp3</song>
    &lt;song>music/song4.mp3</song>
&lt;/playlist>
</pre>


Using the player
-----------------------------------

In your HTML document, make sure you:

1. Always allow script access to the Flash object.
2. Set flashvars to
    - "vpls=PATHTOPLAYLIST&vsng=" if you would like it to initially be loaded
      with a playlist, replacing PATHTOPLAYLIST with the path to your XML
      playlist file, or
    - "vpls=&vsng=PATHTOSONG" if you would like it to initially be loaded
      with a song, where PATHTOSONG is the path to the audio file.

Example player embed code:

<pre>
&lt;object width="550" height="550" id="player">
    &lt;param name="movie" value="player.swf">
    &lt;param name="allowScriptAccess" value="always" />
    &lt;param name="FlashVars" value="vpls=playlist.xml&vsng=" />
    &lt;embed src="player.swf" width="550" height="540" allowScriptAccess="always"
        name="player" FlashVars="vpls=playlist.xml&vsng=">&lt;/embed>
&lt;/object>
</pre>


Interfacing with the Player using Javascript:
--------------------------------------------

Use the sendCommand(string) callback to tell the player to load a song or
playlist. Command strings are formatted like this:

<pre>
load|enqueue song|playlist &lt;filepath>
</pre>

Examples commands:

- Loading a playlist: <pre>load playlist list.xml</pre>
- Enqueueing a song: <pre>enque song music.mp3</pre>

Example Javascript:

<pre>
function sendCommand(command)
{
    var player;
    
    if (window.document["player"])
        player = window.document["player"];
    else if (navigator.appName.indexOf("Microsoft Internet") == -1 && document.embeds && document.embeds["player"])
        player = document.embeds["player"];
    else if (navigator.appName.indexOf("Microsoft Internet") != -1)
        player = window["player"];
    else
        alert("Error: Could not communicate with the player.");
    	
    player.sendCommand(command);
}

player.sendCommand("load playlist playlist.xml");
</pre>

Known Issues and Annoyances:
-----------------------------------

- .wav files don't work with the player
- pausing and unpausing advances the playhead by aproximately one second
- the equalizer is non functional