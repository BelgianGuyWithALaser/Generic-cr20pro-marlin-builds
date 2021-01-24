# Generic-cr20pro-marlin-builds
Generic skr 1.3 / 1.4 / 1.4 turbo builds

If this project help you reduce time to develop, you can give me a cup of coffee :)
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](dassie1993@msn.com)

Do Note, these have not been tested by me since I have a complete different set-up (wich doesn't allow me to test this) 
I'm in no way responsible for any damage to your machine (I'm giving safety pre-cautions and ways to test it out safely. Failure to avoid them could potentially end up in a nozzle bump in your bed)

These are generic made configs for use with the creality CR20 Pro machines for SKR1.3/1.4 and 1.4 Turbo.
You won't need to sift through 100's of lines of code.
However, you will need to adjust only a few lines for your set-up. I have done the hard work already for you guys to enjoy.

Make sure to download the whole folder of the motherboard you are using.
You have 2 choises for your hotend cooling fan. You can either plug it in a fan header port so it's on 100% of the time. Or you snip off the JST connector and connect it to the E1 heater spot.
That way it will start once the hotend reaches 50 Degrees Celcius and Higher. (this will make this fan silent when idle)

You will need to swap the blue and red wires from the servo connector on the BL-touch. and add the black and white cable with the 2pin JST connector from the BL-touch into the Z- endstop location.
You will need to snip 1 of the locks so it fits. You need the bottom 2 connected. (when the drivers are on the top side) 
You will also need to flip exp1 and exp2 180Degrees in their connectors. the easiest way is to snip off the lock and place them inside.

Measure your BL-touch offsets, you will need to fill these in the code. (search for NOZZLE_TO_PROBE_OFFSET in config.h) hit recompile.

place the .bin file Location at : SKR"Version"\Marlin-bugfix-2.0.x\.pio\build\LPC176X on your SD-card and boot up.
You should see the BL-touch do a self test, the scvreen should light up. and you should see the marlin bootscreen.

DO NOT HOME ANY AXIS YET.
Move your bed and X-carry away from the endstops. connect with pronterface (or octoprint or cura or anything with a terminal)
Send a M119 and see what the results are from the endstops. (https://marlinfw.org/docs/gcode/M119.html)
X and Y should mention open. if they mention triggered you would need to invert that in the firmware. (search for X_MIN_ENDSTOP_INVERTING and invert which one mentions triggered)
Next place the bed / carry against the endstops so they should be triggered and send a M119 again. If they mention triggerd they are working correctly. If they haven't check your wiring it's not getting a signal.

Now turn your Z-coupler so your X-gantry is up high from your bed. (safety reasons will be explained in a bit)
Now you can press the "home all" in the motion menu. it should home X and Y perfectly fine. then it should move the probe to the middle of the bed and deploy.
It will start to move Z-downwards. push the probe pin in with your finger, it should stop moving Z downwards and wants to make a "second slower" confirmation.
If it doesn't stop, turn off your printer and check your wiring (it's possible the black/white cable from the bl-touch needs to swap positions with each other)
retry again and it should be working that way.
If it triggers the BL-touch, you can home again and let it go to the bed.

Next thing up on your list is a PID-tune, both for the hotend and for the bed.
M303 E0 C10 S200 -> this is 10 cycles to 200 Degrees for your hotend. Note down the value's (copy paste them in word or anything that would allow you to save text)
Do the same for your bed : M303 E-1 C8 S60

Now move your X and Y to 0 and set Z to 1.
Check at what X and Y value you're at the start of your bed. (it's possible to have your endstop 10mm away from the starting point of your bed)\
Write these 2 value's down aswell. we will fill those out in the firmware aswell, so you will have your true 235x235 printing area.

Once you have those value's you wan't to set them in your firmware again (so you won't lose them at all)
Search for PID inside the config.h (and adjust them with your numbers.) 
for the X and Y offset, search for X_MIN_POS and fill the value in there. don't forget to make it a negative value. if you had X=5 before you reached the first edge of your bed, the value in your firmware is -5.
This is to make the firmware know that the corner of your printbed is the origin (0.0) point.
Recompile and load it back to your SD-card.
Power cycle and you should be good to go now.

Happy Printing
