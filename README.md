## Crowd Chase EL

Is a simple EL wire controller to be used by a group of people wearing EL Wires.
Each person sets which number they are in a sequence by pressing a push button.  They also set the total number of people in their
group using the other push button.  So, if you had 3 people.  Someone would be 1 of 3, 2 of 3, and 3 of 3.

The controller then turns on their EL Wire whenever it's their turn in the sequence.

The circuit uses a basic 8 bit PIC microcontroller (sorry arduino peeps)... The board design uses all through hole components to make it easy for novices to solder.

I designed this controller for Burning Man 2013.  We all invested in some EL Wire (for safety reasons)... and thought it'd be
really cool if we blinked in succession.  Plus it'd had been a while since I had done a hardware project.


### Quick Info ###
* BOM Cost: $8.60 in Qty 10 (not including the PCBs)
* Compiler: Microchip XC8 (but any compiler for PIC16 should be fine)
* PCB Layout: Eagle v6
* License: Creative Commons Share a Like
* Microcontroller: PIC16F1826
* IDE used: MPLABX
* Battery Life: Over a week on a single CR2032.


## Where's the source code? ##
If I haven't posted it here yet, just shoot me an email (see github profile).  I reused a project that I have to strip some of the files
out of before I can release it as open source.


### Project Status ###
I'm looking for people who want to take this project to the next level.  I started this as a project for burning man 2013.
I got it to work but there are some improvements that could be made.

__I'd be willing to ship out some of the controller boards I made to someone that would like to continue this project.__


### Version 1 ###
![PCB PHOTO](https://raw.github.com/blak3r/crowd-chase-el/master/pcbPhoto.jpg)

Well, I made quite a few mistakes on the version 1 board which i'll outline here.  I had very little time to get this board
out before my deadline.  The board works it just required assembling things slightly different and in soldering a resistor
here and there to another part because there wasn't a pad.

#1 - I should have used optically isolated triacs!  You have to use a optically isolated triacs when you're using battery powered circuits.  Basically, you should just always
use optotriac's unless you are certain you don't need them.  If you don't, then you don't have a common ground reference between
the AC output of the inverter and the battery powered controller pcb.  This was a pretty embarrassing mistake.  I was pretty
confused how it was going to work in the first place and shouldn't have just "trusted" that the SparkFun design would work in
my situation.  The sparkfun board basically assumes that you will be powering your inverter and your controller board off one or
more DC wall worts, which in general, will share a common ground (your earth ground of your house)...

I came up with a fairly clever workaround... based around parts I had laying around.  I had some N-channel and p-channel mosfets that worked out.  I got lucky
as I added pads for a second TO-92 transistor along with .1" header to act as a breakout.  So, what I ended up doing is rather then switching the AC voltage from the invertor,
I switched the battery voltage for the inverter using a load switch.  I think this approach is actually probably better anyway as with some invertors
it's bad to keep them running without any load (ie EL Wire) connected to them.

#2 - The schematic has a PIC18F part on it but the target is actually a PIC16F chip.  I used an 18 pin microchip library that was in the sparkfun library but the pins
don't match 100%... so the IO pins used in the source code don't match the IO pin numbers on the schematic.

#3 - I forgot to put pull ups on one of the switches and it was connected to an PORTA io pin which can't have weak pullups enabled :(.

#4 - The voltage regulator pins didn't match the one I used in the library.  As a result you have to rotate the part slightly
 when assembling.

#5 - My 1.8V regulated output is NOT enough for the LEDs i picked.  The LEDs don't blink very brightly which makes
it really annoying to work with outdoors when the sun is up.

#6 - Some of the boards are a bit unreliable.  I found the PIC going into failsafe oscillator mode from time to time.  I'm not sure
exactly what the cause of this is... Perhaps the crystals are finicky to loading... perhaps it's the 1.8V operating voltage...
perhaps it's the fact that my boards didn't have solder mask on them and I never completely potted the boards as I ran out of
time and things shorted out.  I'd definitely recommend getting solder mask!

#7 - My ICSP pads for the Tag Connect didn't seem to work.  I suspect this was due to the limited number of drill sizes
that PCBExpress offers on their standard service.  The holes for the line up pins were WAY to large.  As a result, I ended up


### Other Critiques ###

1. The board could be a LOT smaller... I decided to go through-hole to make the part very easy to assemble... but it really adds a lot of bulk.
2. I wasn't very happy with the push buttons.  They take up quite a bit of board space.  I'd also like to see a switch with recessed actuators to prevent
the possibility of being pressed accidently in pocket.  Note: this didn't actually happen to us in practice... probably because of the bulky
connectors on the edges of the board.
3. Syncing is a bit difficult unless you have a group of patient people.  There were two ways to sync A) by everyone releasing a button at the same time and B) by connecting the boards
together with two wires.  The pressing a button can be difficult with a lot of people... It's hard to have everyone release the button "on 3". 1-2-3.
The wire method is also difficult when you have people who have already put their costumes on etc.



### Alternate Sync Methods ###

The way I kept the boards in sync was just by using timers in the micro and having them "reset" when the sync action occurs.  This works fairly well for
an evening or two.  Depending on where you're using it that might not be an issue.  But, getting 7 people together at Burning Man is pretty much
impossible and if you do you'll run into the issue mentioned above.  To improve the accuracy of the clocks, I used what I thought was a fairly
precise crystal and used an LDO on board to eliminate changes in the operating voltage from PIC to PIC.

What would be ideal is some sort of low power zigbee node mesh controller... where master and slaves are dynamically negotiated.  The master would then send
out a pulse signal to the slave nodes when it was their turn.  This would make the whole thing much easier to use as a group.
As people come in range they'd be assigned a position automatically.
If two people went off on their own, then they'd start blinking back and forth instead of having dead time where neither is blinking.
It would however make for some pretty complex software and increase the BOM Cost probably about $5-$10.  The possibilities
would be pretty much endless though... You could probably even do some smart algorithms to determine where people were in relation to one another.
That way you could always keep the blink pattern going from left to right even when people moved around.

A simpler approach would be a simple RF transmitter / receiver pairs.  In this topology you'd have a single master controller which would send
pulse signals to the slave boards to keep them in sync.  You can get these for about $5 but what I didn't like about that was then you
always had to have a "master" board in range of the "slave" board and they'd drain the coin cell batteries pretty fast.  The zigbee stacks have battery
friendly "time slots" in which all communications occur so the RF controllers could be off most of the time.



### Contact ####
I'm very interested in seeing this project go somewhere.  I'm afraid I don't have too much time to keep working on it nor an event
to use it at for at least a year... so send me an email if you're interested.  My email is in my github profile.