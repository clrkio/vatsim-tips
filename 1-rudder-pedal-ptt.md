# Using rudder pedals as dual push-to-talk pedals

When acting as an Air Traffic Controller on VATSIM, it's useful to be able to type while talking on the radio or on the virtual land-line with other controllers.
This is difficult if the push-to-talk (PTT) keys are on the keyboard, especially if the PTT keys are special keys, like CTRL or SHIFT, which change the meaning of other keyboard keys.
It would be great to be able to use your feet to activate the PTTs, while using your hands to type on the keyboard.
Many people use external pedals like [these](https://www.delcomproducts.com/webpage.asp?id=32), but they can be expensive and take up space.
On the other hand, many ATCs on VATSIM are virtual pilots as well, and often own rudder pedals as part of their simulation gear.
This tip will show you how to use those rudder pedals as dual push-to-talk pedals when controlling on the network.

To do this, we will be using [AutoHotKey](https://www.autohotkey.com/) to send key presses when the left and right pedals' brake function is activated

## How I use this

I use the toe brakes on my left and right pedals to activate PTT for my radio and Teamspeak, respectively. I usually have TeamSpeak on the right side of my computer, so I use the right toe brake for Teamspeak, and the left toe brake for AudioForVatsim.

## Things to know

For some reason, Teamspeak is very particular about key presses. 
AutoHotKey does not seem to be able to send a key press / unpress action to Teamspeak if it is the currently focused window. 
Therefore, you must click a different window after changing channels in Teamspeak, otherwise your PTT will not activate or deactivate.
If you forget, and realize you aren't transmitting in Teamspeak, or are still transmitting when you don't intend to be, just click another window, then unpress and press your pedal again.
I use the blue circle next to my name turning light blue to know that I am transmitting on Teamspeak.
If you figure out a solution to this problem, let me know!

## Steps

### Step 1: Install AutoHotKey

Go to https://www.autohotkey.com/, and download the latest version of AutoHotKey.
The Express Installation should work for most people, but if you need to customize, feel free.

### Step 2: Unplug all flight control surfaces except your rudder pedals

If you have multiple flight control surfaces, like rudder pedals, a yoke, and a throttle, unplug everything except the pedals.
The other devices look like a joystick to AutoHotKey too, and we don't want to get confused about which joystick number to use.


*Note:* The joystick number should stay consistent even if you unplug the pedals, but I haven't tested this thoroughly.
If your pedals stop working as a PTT after plugging in other devices, or after unplugging and re-plugging the rudder pedals, let me know. 


### Step 3: Run the Joystick Test Script

When you run AutoHotKey, it will open up the documentation page. 
On the left hand side, click the `Script Showcase`, and then go to the `Joystick Test Script`.
Click on `Show code`, and then run it by clicking `Open`.
This will run a sample AutoHotKey script that makes a box next to your mouse that shows the joysticks you have connected, the axes they have, and the current value of the axes.


It looks like this once it's running:

// TODO(wclarke): insert image

### Step 4: Find your joystick number

The first piece of information we need about the rudder pedals is the joystick number it was assigned.
The joystick number is at the end of the first row, in parenthesis.
Write this number down.

On my computer, the rudder pedals are joystick number 3.


### Step 5: Find the name of each axis, and the range of values

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


### Step 6: Pick your pedal ranges
 
For both axes, pick what range of values you want the key to be pressed during.
For me, the toe brakes have a value of 0 when fully pressed, so I use 0-30 as the values that mean the PTT button should be pressed.
Add that to your table.

Axis | Letter | Range | Activation Range
-----|--------|-------|--------
Left toe brake | Y | Not pressed: 100, Fully pressed: 0 | 0-30
Right toe brake | X | Not pressed: 100, Fully pressed: 0 | 0-30
Rudder | Z | Full left: 0, Neutral: 50, Full right: 100 | n/a

### Step 7: Make a blank AutoHotKey script

Create a new AutoHotKey script on your Desktop (or wherever else you want it - remember that you have to start it every time you control). 
Do this by right-clicking, selecting New, then selecting AutoHotKey Script. 
This will make a new blank AutoHotKey script. 
You can name it whatever you want.

### Step 8: Copy the AutoHotKey script

Right click on the icon for your new AutoHotKey script, then select `Open with`, then select `Notepad`.
There will already be some lines in the script. Leave them there, and paste this after them: 

```
; EDIT THESE VALUES TO SUIT YOUR PREFERENCES

LeftKey  = Pause
RightKey = ScrollLock

LeftJoystickAxis = 3JoyY
RightJoystickAxis = 3JoyX

LeftPressedLow   := 0
LeftPressedHigh  := 30

RightPressedLow  := 0
RightPressedHigh := 30


; DON'T EDIT BELOW HERE

HoldingLeft := false
HoldingLeftPrev := false

HoldingRight := false
HoldingRightPrev := false

#Persistent
SetTimer, WatchAxis, 200
return

WatchAxis:
HoldingLeftPrev = %HoldingLeft%
HoldingRightPrev = %HoldingRight%

GetKeyState, LeftBrake, %LeftJoystickAxis%
GetKeyState, RightBrake, %RightJoystickAxis%

if (LeftBrake >= LeftPressedLow && LeftBrake <= LeftPressedHigh) {
    HoldingLeft := true
} else {
    HoldingLeft := false
}

if (HoldingLeft != HoldingLeftPrev) {
    if (HoldingLeft == true) {
	Send {%LeftKey% down}
    } else {
	Send {%LeftKey% up}
    }
}

if (RightBrake >= RightPressedLow && RightBrake <= RightPressedHigh) {
    HoldingRight := true
} else {
    HoldingRight := false
}

if (HoldingRight != HoldingRightPrev) {
    if (HoldingRight == true) {
	Send {%RightKey% down}
    } else {
	Send {%RightKey% up}
    }
}

return
```

### Step 9: Edit the AutoHotKey script - Keys

AutoHotKey has the ability to virtually "press" any key on your keyboard, and many not on your keyboard.
Pick a key from [this list](https://www.autohotkey.com/docs/KeyList.htm) to be pressed when the left and right pedals are pressed.
Put the name of the key from the list on the right side of the equal sign next to the `LeftKey` and `RightKey` variables, where `Pause` and `ScrollLock` currently are.

I use keys like Pause and Scroll Lock, which are on the keyboard, so I can use my keyboard if there is a problem, but don't do anything special or cause a character to be typed.

### Step 10: Edit the AutoHotKey script - Joystick Axes

Change the variables named `LeftJoystickAxis` and `RightJoystickAxis` if necessary. 
Substitute your joystick number for the existing number `3`, and your joystick axis letters for the existing letters `X` and `Y`

### Step 11: Edit the AutoHotKey script - Ranges

Adjust the high and low ranges for each axis if necessary. If you want the button to be pressed when the joystick value is between 70 and 100, change `LeftPressedLow` to 70, and `LeftPressedHigh` to 100. Do the same for the right side.

### Step 12: Run the script

Make sure to save your changes to the AutoHotKey script.
Then, find the icon for the script, and double-click it to run.
Make sure Teamspeak and AudioForVatsim are configured to use the same Push to Talk keys as you made the script press.
