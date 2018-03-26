This is a very basic repository for my IBM 3161 ASCII terminal font.

I used GNU Unifont as a base because it looks somewhat similar and covers a lot
of Unicode above the basic ASCII set my IBM terminal supports.

![Screenshot of the ASCII set on my terminal](http://i.imgur.com/f2eG8H8.png)

## Documentation

### Introduction
This repository contains a bitmap version of the font as well as a (slightly 
less complete) truetype version created through GNU Unifont's "tracing" program 
which it uses to create its truetype versions. The truetype covers 0x00 through 
0x7F (the basic ASCII set), where I have extended the font to cover through 
0xFF in the bitmap versions (I generated the truetype before I added 0x80-0xFF 
into the bitmap, and I don't have access anymore to the scripts I made to
convert to a truetype. They are too much effort for something I rarely use, and
all of the essentials are still in the TTF anyway).

I also have included the Fontforge Spline Font Database (SFD) files, which
are uncompressed text (and thus pretty big; sorry). If you just want to use the
font, I therefore suggest downloading the zip on the 'releases' page instead of
cloning the repo.

#### Formats
Since I first created this font, I have been finding more and more formats I can
convert it to! Whenever I find another format to port to, I do so and then add
a commit here. Most recently, Minecraft ,Rockbox, and an
X11 emacs-specific font (to fix strange rendering issues) have been added.

This font is also available in a Minecraft 1.11-and-up compatible resource pack.
With minor tweaking it should also work for earlier and future versions.
Conveniently, Minecraft uses GNU Unifont for unicode text, which is also the
base for my font - this made it simple to add by modifying some tools included
with the Unifont source distribution to export the font.

Instructions to install the Minecraft version of the font are under the
`minecraft/` directory.

Additionally, a Rockbox font has been added. Rockbox is an alternative firmware
for many mp3 players. It has been tested on my iPod Classic. Instructions
are of course in the `rockbox/` directory.

![Photo of font on an iPod running Rockbox](https://i.imgur.com/UsE8YDw.jpg)

## Installation
This depends on your platform, but I will try to lay out what works where.
For some exotic formats (e.g. Minecraft, Rockbox) the directions are in the
ports' respective subdirectories.

##### Why install both the bitmap and TrueType versions?
On Unix/Linux and Windows platforms the bitmap versions benefit from not
getting any software subpixel rendering attempted on them like the TrueType
font does. This is probably true in OS X as well, but I only tested the .dfont
version of the font in OS X on account of not actually having regular access to
OS X. (the dfont is a bitmap-only font, so it probably won't be subpixel
rendered.)

### Windows Installation
On Windows, you will want the windows bitmap .fon file and/or the TrueType font
(.ttf). They can be copied manually to `C:\Windows\Fonts`, or installed from
the font previewer.

### Unix/X11 Installation
In Unix and Linux distros with X11, you'll want the OpenType bitmap (.otb)
and/or the TrueType font (.ttf).

There is also a PCF font (old-school X11 font) that it may be a good
idea to install for programs like `xterm` and the GUI version of `emacs`.

Installing all three versions (TTF, OTB, and PCF) lets you use the font with
nearly any program on a Unix system. The TTF and OTB can be selected with most
'font chooser' GUI programs, while the PCF is useful for traditional X11
programs like `xterm` and is usually set through a text configuration file
like `~/.Xresources` or `~/.emacs`.

I think this will be the same on the BSD's with X11 as it is for Linux distros,
since I believe they share a codebase for their X servers and FreeType.

#### X11 OTB and TTF Installation
On my Debian system, I made the directory `~/.fonts` and copied
IBM3161-bitmap.otb and IBM3161.ttf to it. If you don't want/need both versions,
just copy one or the other. Java programs using AWT/Swing don't seem to work
with X11 bitmap fonts on Unix systems, though (at least in OpenJDK), so I had
to keep the TrueType version around for Java programs.

The OTB is in the directory `opentype-bitmap/`, and the TTF in `truetype/`.

The PCF's are the only fonts in the `X11/` directory, since they are the ones
standardized by the X consortium and are the only technically X11-specific fonts
here.

##### If the OTB bitmap version isn't working
In Debian, Ubuntu, and possibly other distributions, FreeType has bitmap fonts
disabled by default for some silly reason.

In Debian, this is fixable by deleting 
`70-no-bitmaps.conf`: 
````
rm /etc/fonts/conf.d/70-no-bitmaps.conf
````
You may also want to consider running `dpkg-reconfigure fontconfig-config` and 
changing the setting there. Then you might also have to run `dpkg-reconfigure 
fontconfig` after that, according to random people on the internet. This is
not how I have always done it, but it would make sense and be a more 'canonical'
solution than my own.

One can also just delete `70-no-bitmaps.conf` and copy
`/usr/share/fontconfig/conf.avail/70-yes-bitmaps.conf` to `70-no-bitmaps.conf`,
and then use `chattr +i 70-no-bitmaps.conf` to make sure updates don't
overwrite my setting (by setting the 'immutable' flag).
This is likely sub-par, but that's just how I have always done it (tm).

#### X11 PCF Installation
The instructions for installing the X11 PCF font are in the
[X11/README.md](X11/README.md)
file.

### Mac OS X ('MacOS') Installation
On Apple systems (OS X), you'll want to use the '.dfont' file. If that fails,
try the .ttf or the .pt3 (PS type 3). IIRC, the installation process was
relatively intuitive.


## How I made it
I screen-scraped the font from my IBM 3161 by hand via its built-in "test"
mode, where it prints out its entire ASCII character set on a single screen. As
a result, I cannot guarantee it to be 100% accurate in terms of spacing between
characters, but I can say that I personally have yet to notice anything wrong
with it. I have used it in my terminal emulator exclusively for five months now.

You can find some of the images I used [here](http://imgur.com/a/XHyDb) (though
I have others that I did with a different amber CRT; I still need to dig those
files up).

As such, I am as confident as one can be when using guesswork and (to an extent)
tracing the letters in CAD and transforming my photos of the screen to correct
for the CRT's curvature. I am certain that the vertical pixels are accurate, as
I can make out individual scans of the electron beam in my photos. I am
relatively certain that the horizontal pixels are accurate based on common
sense, the fact that most characters would look wildly incorrect if shifted
over a pixel, and from some trigonometry I did using the CAD tracings. Character
0x7F is a checkerboard pattern, which made it easy for me to find out what the
aspect ratio of each 'pixel' is on the CRT near the center of the display.

![Photo of IBM 3161 terminal with amber CRT playing the text-based adventure gam
e, Zork](http://i.imgur.com/MkboCJs.jpg)

Still not done
--------------
My font does not currently supply most of the non-ASCII "Extended" characters
that are in the real 3161 (characters above `0x7F`). Even those characters that
have unicode equivalents do not have them associated with the same 'numbers'
as they have in the terminal, and I am not clever/talented/obsessive enough to
map those all out since I never use them. Additionally, some characters do not
have a unicode equivalent that I am aware of at all.



If anyone wants to add them, I will point you to [the photos of the terminal's
screen in its 'testing' mode,](http://imgur.com/a/XHyDb) which contains a full
printout of all of the characters the terminal is capable of displaying. Feel
free to send me a pull request!

Licensing
---------

These font files are licensed under the GNU General Public License (where
applicable), either version 2 or (at your option) a later version, with the
exception that embedding the font in a document does not in itself constitute a
violation of the license. The full terms of the license are in GNU Unifont's
[LICENSE.txt](http://unifoundry.com/LICENSE.txt). This license is inherited
because I have based my font on GNU Unifont for nearly all non-ASCII characters.

If you wish to use only the glyphs that I created (0x00-0x7F in the truetype, or
0x00-0xFF in the bitmap), those glyphs are licensed (at your option) under the
GNU GPL version 2 or later (with the aforementioned exception regarding
embedding), the 
[SIL Open Font License, version 1.1](http://scripts.sil.org/cms/scripts/page.php?site_id=nrsi&id=OFL#5667e9e4), 
or the 3-Clause BSD License, which is reproduced
below:


Copyright 2017 Wyatt Ward

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this
list of conditions and the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright notice,
this list of conditions and the following disclaimer in the documentation
and/or other materials provided with the distribution.

3. Neither the name of the copyright holder nor the names of its contributors
may be used to endorse or promote products derived from this software without
specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
