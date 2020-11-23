# Using rudder pedals as dual push-to-talk pedals

When acting as an Air Traffic Controller on VATSIM, it's useful to be able to type while talking on the radio.
This is difficult if the push-to-talk (PTT) key is on the keyboard, especially if the PTT key is a special key, like CTRL or SHIFT, which changes the meaning of other keyboard keys.
It would be great to be able to use your feet to activate the PTT, while using your hands to type on the keyboard.
Many people use external pedals like [these](https://www.delcomproducts.com/webpage.asp?id=32), but they can be expensive and take up space.
On the other hand, many ATCs on VATSIM are virtual pilots as well, and often own rudder pedals as part of their simulation gear.
This tip will show you how to use those rudder pedals as dual push-to-talk pedals when controlling on the network.

To do this, we will be using [AutoHotKey](https://www.autohotkey.com/) to send key presses when the left and right pedals' brake function is activated.

## Step 1: Install AutoHotKey

Go to https://www.autohotkey.com/, and download the latest version of AutoHotKey.
The Express Installation should work for most people, but if you need to customize, feel free.

## Step 2: Unplug all flight control surfaces except your rudder pedals

If you have multiple flight control surfaces, like rudder pedals, a yoke, and a throttle, unplug everything except the pedals.
The other devices look like a joystick to AutoHotKey too, and we don't want to get confused about which joystick number to use.


*Note:* The joystick number should stay consistent even if you unplug the pedals, but I haven't tested this thoroughly.
If your pedals stop working as a PTT after plugging in other devices, or after unplugging and re-plugging the rudder pedals, let me know. 


## Step 3: Run the Joystick Test Script

When you run AutoHotKey, it will open up the documentation page. 
On the left hand side, click the `Script Showcase`, and then go to the `Joystick Test Script`.
Click on `Show code`, and then run it by clicking `Open`.
This will run a sample AutoHotKey script that makes a box next to your mouse that shows the joysticks you have connected, the axes they have, and the current value of the axes.


It looks like this once it's running:

// TODO(wclarke): insert image

## Step 4: Find your joystick number

The first piece of information we need about the rudder pedals is the joystick number it was assigned.
The joystick number is at the end of the first row, in parenthesis.
Write this number down.

On my computer, the rudder pedals are joystick number 3.


## Step 5: Find the name of each axis, and the range of values

Rudder pedals have 3 axes: left toe brake, right toe brake, and rudder.
Each of these axes are assigned a letter, which varies from one device to another.


One by one, press on each of the axes of your rudder pedals, and see which number value changes on the second line.
For each pedal axis, write down which letter it is assigned in the box, and what the numbers are when not pressed and fully pressed.

For my rudder pedals, the table looks like this:
Axis | Letter | Range
-----|--------|-----
Left toe brake | Y | Not pressed: 100, Fully pressed: 0
Right toe brake | X | Not pressed: 100, Fully pressed: 0
Rudder | Z | Full left: 0, Neutral: 50, Full right: 100









