---
title: The shell and the terminal
weight: 20
blackfriday:
 fractions: false
---


![](/v1/images/prerequisites/linux/shell-and-terminal/terminal-icon.png)

A [shell][shell] is a user interface for access to an operating system's services. Most
often the user interacts with the shell using a
command-line interface ([CLI][cli]). The [terminal][terminal-emulator] is a program that opens a graphical window and lets you interact
with the shell.

[shell]:https://en.wikipedia.org/wiki/Shell_(computing)
[terminal-emulator]: https://en.wikipedia.org/wiki/Terminal_emulator 
[cli]: https://en.wikipedia.org/wiki/Command-line_interface 

## Background 

Originally, a computer terminal was an electronic or electromechanical hardware
device used for entering data into, and displaying data from, a computer or a
computing system. The terminal of the first working programmable, fully
automatic digital [Turing-complete][turing-complete] computer, the [Z3][z3] (1941), had a keyboard and a
row of lamps to show results.[^computer-terminal]

[z3]: https://en.wikipedia.org/wiki/Z3_(computer) 

[turing-complete]: https://en.wikipedia.org/wiki/Turing_completeness

[^computer-terminal]: https://en.wikipedia.org/wiki/Computer_terminal

Early computers where huge machines taking up a lot of space. Commonly a system
consisted of multiple cabinets, for example one cabinet for the main processor
unit, one or more cabinets for tape drives, one cabinet for each disk drive, one
cabinet for a punched card reader and one cabinet for a high speed printer. In the
below image, a Univac 9400 system (1967) consisting of multiple cabinets is
shown.

{{% figure
   src="/v1/images/prerequisites/linux/shell-and-terminal/univac-9400.jpg"
   caption="A Univac 9400 mainframe computer data center on display in the Techikum29 museum."
   source="http://www.technikum29.de/en/computer/univac9400"
   type="Photograph"
   by="technikum29"
   bylink="http://www.technikum29.de/en/"
   attr="cc by-sa 2.5"
%}}


<!-- More info on Univac 9400  http://www.technikum29.de/en/computer/univac9400 --> 

## Teletypewriter (TTY)

Early user terminals connected to computers were electromechanical
teleprinters or teletypewriters (TeleTYpewriter, TTY). In the above image of the
Univac 9400 system, the cabinet marked UNICAC 9400 is the main
processor cabinet. The terminal is the machine looking like a huge typewriter
placed on the desk to the left of the main processor cabinet. Another example of an early terminal is
the [Teletype Model 33 ASR][teletype-33-asr] (1963) shown below.

[teletype-33-asr]:https://en.wikipedia.org/wiki/Teletype_Model_33 
{{< figure 
    src="/v1/images/prerequisites/linux/shell-and-terminal/teletype-model-33.jpg"
    maxwidth="600px"
    source="https://commons.wikimedia.org/wiki/File:ASR-33_at_CHM.agr.jpg"
    caption="A model 33 ASR terminal from the Teletype Corporation on display at the Computer History Museum, Mountain View, California, USA."
    type="Photograph"
    by="Arnold Reinhold"
    attr="CC BY-SA 3.0"
    
>}}

## Video display terminal

As technology improved, teleprinter terminals was replaced by video display 
terminals. One example of such a video display terminal is the [DEC VT100][vt-100] (1978)
shown below.

[vt-100]: https://en.wikipedia.org/wiki/VT100 

{{< figure
    src="/v1/images/prerequisites/linux/shell-and-terminal/DEC-VT100-terminal.jpg"
    maxwidth="500px"
    source="https://commons.wikimedia.org/wiki/File:DEC_VT100_terminal.jpg"
    caption="A DEC VT100 terminal."
    type="Photograph"
    by="Jason Scott"
    attr="CC BY 2.0"
>}}


Note that the DEC VT100 terminal shown above is not a computer. The DEC
VT100 terminal was
only used for input and output to and from a connected computer. In the below image [DEC VT52](https://en.wikipedia.org/wiki/VT52) video terminal (1974) is connected to a PDP 11/55 computer (1975). 

{{< figure 
    src="/v1/images/prerequisites/linux/shell-and-terminal/DEC-VT52-terminal-PDP-11-55.jpg"
    maxwidth="600px"
    caption="A DEC VT52 video terminal connected to a PDP 11/55 computer."
    source="http://www.bejaardecomputers.nl/index-en.html"
    type="Photograph"
    attr="courtesy of"
    by="House for Retired and Aged Computers"
    bylink="http://www.bejaardecomputers.nl/index-en.html"
>}}

<!--
{{% figure
           src="/images/prerequisites/linux/shell-and-terminal/pdp-11-40.jpg" 
           maxwidth="450px"
           caption="A PDP-11/40 computer as exhibited in Vienna Technical Museumwith with the processor at the bottom and a TU56 dual DECtape drive is installed above it."
           type="Photograph"
           source="https://commons.wikimedia.org/wiki/File:Pdp-11-40.jpg"
           attr="CC-BY-SA 3.0"
           by="Stefan Kögl"
           bylink="fo"
%}}
-->

## Terminal emulator

A terminal emulator is a program that emulates a video terminal within some
other display architecture.[^terminal] Today, the term terminal is
often used synonymously with a terminal emulator running a [shell][shell].


[teleprinter]: https://en.wikipedia.org/wiki/Teleprinter
[^terminal]: [http://superuser.com/a/144668](http://superuser.com/a/144668)

