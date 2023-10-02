# fcrameri-xmgrace
Shell functions to allow use of Fabio Crameri's color palettes with xmgrace. Plus, general description of how to change available colors in xmgrace.



(last updated Oct 2 2023)

ABOUT

These are three shell functions written to apply Fabio Crameri's colour maps[1] to xmgrace. They are written to work with the colour maps version 8.0.0 (June 14 2023), and compatibility has not been checked with any other version.

These color maps are designed to be readable equally well by people with and without color vision deficiencies, whether printed in black & white or in color. They're also perceptually uniform, meaning each of the colors in a given palette will appear equally intense -- none will overshadow the others.

They're easily available for use in most common plotting programs, but not in xmgrace. I wrote these scripts so I could use them with xmgrace. Hopefully they'll help you too.

I do not intend to maintain this code. But given how unexpectedly difficult I found it to figure out color mapping in xmgrace, I hope this README will, additionally, be more broadly useful to others facing the same issue.

This code is distributed under a GNU AGPLv3 license.



GUIDE TO COLORS IN XMGRACE

Much help came from Germain Salvato Vallverdu[6].

It's relatively easy to choose colors in xmgrace (described in USE below). However, those colors must be selected from a limited set of available options. And changing those defaults is not clearly described in any manual.

It turns out they're defined in a file called Default.agr, which I found in my /opt/local/lib/grace/templates/ folder. But I've seen it described as being elsewhere, so no guarantees where you'll find yours.

You may, however, override that file with an agr file in ~/.grace/templates/. If you do not have that folder path yet, you should create it. You can then copy your Default.agr to it, or simply move over the one I've include in this repository, which is the one I found on my computer.

If you put an .agr file in ~/.grace/templates/ (this .agr file must be called "Default.agr"!), it will automatically override the other one. And since this new one in ~/.grace/templates is not write-protected, it's far more convenient to play around with it.

Color definition lines in that file will have the form

@map color 0 to (255, 255, 255), "white"
@map color 1 to (0, 0, 0), "black"
@map color 2 to (140, 1, 115), "hawaii 1"

and the script I wrote simply deletes all lines with "map color" in them from that file, then writes out new color definition lines as specified by the user.

These colors may then be selected in an options file, as described in USE below.



SETUP INSTRUCTIONS

0. Download Fabio Crameri's color palettes from [2]. Put them somewhere that makes sense. Then make a new folder (I called mine "fcrameri-colors") and copy to it the .txt color palette file from each folder. You may do this with `cp */*.txt /path/to/fcrameri-colors`, if you're sitting in the folder that contains all the color palette folders.

0a. This new "fcrameri-colors" folder should contain just palette .txt files: acton.txt, bam.txt, bamO.txt, etc.

1. Paste the functions from this repository into your .bash_functions file (or equivalent). Or, place the .bash_xmg_functions file from this repository in an appropriate folder and `source` it in your .bashrc file (or equivalent).

1a. What's a .bash_functions file? -- Your .bashrc file (or, on a mac, .zshrc; other Linux distributions have other ".*rc" files) sets options to personalize your terminal, including things like text color, text size, and even any functions you find personally useful that don't come built in. It's typically recommended that functions go in a file other than .bashrc, to avoid cluttering it. You may call this file anything you'd like. I call mine .bash_fns. To make your .bashrc file aware of it, add the line `source /path/to/.bash_fns` (without the ticks ``, and changing the filepath to be the correct one) anywhere in your .bashrc file. Any time you change your .bashrc file, or your .bash_functions file, your terminal won't be aware of it until you `source` that file again.

2. Both `xmg-fc-col` and `xmg-fc-list-cols` have options that must be set for yourself. These options are at the very top of each one. They may include your Linux distribution (recent MacOS versions are typically zsh), whether or not you are on a mac, the directory with your color palette txt files, or the Default.agr file you'll be changing (see next step). Set these options.

3. Check if you have an .agr file in ~/.grace/templates/. If you don't, create those directories (if needed), then put the given Default.agr file I've included into that folder.

4. `source` your .bash_functions file to start using the functions.



USE

Once your functions have been sourced, you may use them as follows:

`xmg-list`

List the functions and their arguments.


`xmg-fc-list-cols`

List available color palettes. For this function to work correctly, your folder with the color palettes .txt files should have only those files, as it basically just lists the files (minus extensions) in that folder.


`xmg-fc-col palette number`

Concrete example: `xmg-fc-col hawaii 5`
Removes all color definitions from ~/.grace/templates/Default.agr, and writes new color definitions: color 0 is white, color 1 is black, and then the next "number" colors are equally-spaced colors from the specified palette.


`xmgrace data_series -batch options.txt`

You're now all set to graph your data series! If you're trying to graph three series, you should have created at least 3 colors using `xmg-fc-col`. The options.txt file you specified with the `-batch` flag can contain all sorts of useful settings for your plot(s), such as axis labels, legend placement, and -- crucially, here -- the color chosen for each plotted data series. xmgrace automatically begins at color 1, which will be black (including by default, without using these functions). But to use only colors from these palettes, you should start choosing colors starting from 2, the first color defined from the palettes. To do this, you should include line(s) in your options.txt file that look like

s0 line color 2
s1 line color 3
s2 line color 4

for each data series "s#" you're plotting.




REFERENCES

1. https://www.fabiocrameri.ch/colourmaps-userguide/
2. https://www.fabiocrameri.ch/colourmaps/
3. Crameri, F. (2018). Scientific colour maps. Zenodo. http://doi.org/10.5281/zenodo.1243862
4. Crameri, F. (2018), Geodynamic diagnostics, scientific visualisation and StagLab 3.0, Geosci. Model Dev., 11, 2541-2562, doi:10.5194/gmd-11-2541-2018
5. Crameri, F., G.E. Shephard, and P.J. Heron (2020), The misuse of colour in science communication, Nature Communications, 11, 5444. doi:10.1038/s41467-020-19160-7
6. https://gsalvatovallverdu.gitlab.io/post/2013-11-21-configuration-de-xmgrace/
