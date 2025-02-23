# Giga-Beacon
> [!IMPORTANT]
> <h2>This design requires the Beacon H - Normal Variant</h2>

> [!CAUTION]
> <i><b><H3>I am not responsible for any damage to your device, perform these modification at your own RISK!</H3></i></b>

https://github.com/user-attachments/assets/93804f33-2fc6-4fdc-bc21-9b3e94921d5a

<br />
<h2>Things to Consider</h2>
I highly recommend getting your Beacon from Lukes Lab so you can get the 5M USB for the Giga. (Make sure to notify them you want the Type H Normal Variant)
<br />https://www.lukeslabonline.com/products/beacon?variant=49920272597293
<p></p>

<h2>Refefrence Material</h2>
Beacon Install Quick Start Guide - This was used for reference in below configs
<br />https://docs.beacon3d.com/quickstart/ 
<p></p>

<h2>Things to Print</h2>
Download & Print <a href="https://github.com/jranger615/Giga-Beacon/blob/main/STL/Giga%20Beacon%20Adapter.stl">Giga Beacon Adapter.stl</a>
<br />You will need 6 m3x4 Heat Inserts. The LED Relocation bracket insers should installed from the bottom
<img src="https://github.com/user-attachments/assets/a4b1091f-e894-47d4-8b68-01195215e51c"/>
<ul>
<li>ABS(Recommend)</li>
<li>.16 line height</li>
<li>3 Walls</li>
<li>4 Top/Bottom Layers</li>
</ul>
<p></p>

<h2>Hot End Modifications</h2>
<ul>
<li>Remove the Hot End from the Printer(4 M3 Screws, 2 front, 2 back)</li>
<li>Remove the Case by 2 M3 Screws attaching it</li>
<li>Remove the Pobe, 2 screws attach it to the metal casing, then trace the wire to the board and fish the wire out of the casing</li>
<li>Remove the 2 LED Screws and pull it to the side</li>
<li>Use a Dremel with Cut off Wheel to cut the metal casing as shows in the RED Outlines in the picture
<br /><img src ="https://github.com/jranger615/Giga-Beacon/blob/main/photos/Hot-End%20Modification.jpg?raw=true"/>
<img src ="https://github.com/jranger615/Giga-Beacon/blob/main/photos/Hot%20End%20Mod%20Completed.jpg?raw=true"/></li>
<li>Using the cut off wheel Cut the Plastic Cover as shown in the picture
<br /><img src ="https://github.com/jranger615/Giga-Beacon/blob/main/photos/Plastic%20Cover%20Mod.jpg?raw=true"/></li>
<li>Cut a hole in the top of the Plastic Casing big enough to fit the USB Cable End through, it can also be snaked through near the fan.
<br /><img src="https://github.com/jranger615/Giga-Beacon/blob/main/photos/USB-Hole.jpg?raw=true"/></li>
<li>Run the USB through the Cable Chains to the hot end location, fish it through the plastic cover, and down where the LED Use to be</li>
<li>Mount the USB on the Adapter with 2 M3 Screw</li>
<li>Mount the Adapter to the stock Sensor Location using 2 M3 Screw, make sure it is tight fitting against the side metal case near the LED. This helps make sure its flat</li>
<Li>Attach the USB to the Beacon, and secure the beacon to the Adapter with 2 M3 Screw
<br /><img src ="https://github.com/jranger615/Giga-Beacon/blob/main/photos/Beacon-Mounted.jpg?raw=true"/>
<img src ="https://github.com/jranger615/Giga-Beacon/blob/main/photos/Beacon%20Mounted-Front.jpg?raw=true"/></Li>
<li>Re-Mount your Hot End to the Printer</li>
<li> PLug the USB into the Front Port on the Giga</li>
</ul>
<br />
<h2>Beacon Install/Configuration File Modifications</h2>

> [!IMPORTANT]
><b>Before you do this, turn on your printaer and HOME ALL.</b>

<ul>
<li>On your Klipper Screen, Click Settings > Advanced Settings > Root</li>
<li>Follow the Prompts to Root your Klipper and Allow SSH. Note the Username and PW</li>
<li>On your Pc Download Putty - https://www.putty.org/ </li>
<li>Open Putty and under Host Name type in the IP of your Giga. Should be on the right top corner of the Klipper Screen when connected to WIFI.</li>
<li>Click Connect, and log in with the supplied Credentials.   
<i><b><br />Username: Elegoo
<br />Password: giga3dp</li></i></b>
<li>Run these 3 commands 1 at a time, it will take some time to complete the Beacon Klipper Install.
  
  ```ruby
cd ~
git clone https://github.com/beacon3d/beacon_klipper.git
./beacon_klipper/install.sh
```
  
<li>Do not close putty, we will go back to this shortly</li>
<li>Navigate to Fluidd via your web browser using the IP from before</li>
<li>Choose the Configure Icon on the left, and click printer.cfg to started editing. (I recommend you backup this file to be safe before you begin)
<br /><img src ="https://github.com/user-attachments/assets/8ca86775-1220-49ff-ae42-e1b08427cf48"</img>
</li>
<li> Comment Out the PROBE Section Completely with #. It Should look similar to this:
  
  ```ruby
#####################################################################
# 	Probe
 #####################################################################
#[probe]
#pin:^mcu1:gpio11
#x_offset: -14.0
#y_offset: -29.5
#z_offset: 0.0
#speed: 5.0
#samples: 2
#samples_result: median
#sample_retract_dist: 3.0
#samples_tolerance: 0.05
#samples_tolerance_retries:4</li></i></b>
```

<li>Add the following below the commented out Probe Section:
  
  ```ruby
[beacon]
serial: /dev/serial/by-id/usb-Beacon_Beacon_RevH_5BB811315157355957202020FF0E0F24-if00 #This will be REPLACED
x_offset: -1 # update with offset from nozzle on your machine
y_offset: -18 # update with offset from nozzle on your machine
mesh_main_direction: x
mesh_runs: 2
```

<li>Open Up Putty and Run the following Command
 <i><b><br />ls /dev/serial/by-id</li></i></b>
<li>Copy the USB line that contains Beacon, mine for example was <i><b>"usb-Beacon_Beacon_RevH_5BB811315157355957202020FF0E0F24-if00"</b></i></li>
<li>Paste the USB info in the beacon section under Serial:(It Should look like above but <i><b>/dev/serial/by-id/YOUR COPIED USB DATA HERE)</b></i></li>
<li>Find the section [stepper_z]</li>
<li>Comment Out with #:
 
  ```ruby
#endstop_pin:PC11
#position_endstop:0
 ```
  
   <li>Add the Following lines at the end of stepper_z
   
  ```ruby
endstop_pin: probe:z_virtual_endstop # use beacon as virtual endstop
homing_retract_dist: 0 # beacon needs this to be set to 0</i>
 ```

Under [stepper_z1] comment out with #:
  ```ruby
 #endstop_pin:PC10
 ```

<li>Add Under [stepper_z1] :
  
  ```ruby
  endstop_pin: probe:z_virtual_endstop # use beacon as virtual endstop
  ```

<li>Find the Safe_z_home section and make it as follows:
  
  ```ruby
  [safe_z_home]
home_xy_position: 405,205
# speed: 150
z_hop: 3          
#z_hop_speed: 5
  ```

<li> Click Save in the Top Right Corner, then close</li>
<li>Open moonraker.cfg</li>
<li>At the End Add the Following:

   ```ruby 
[update_manager beacon]
type: git_repo
channel: dev
path: ~/beacon_klipper
origin: https://github.com/beacon3d/beacon_klipper.git
env: ~/klippy-env/bin/python
requirements: requirements.txt
install_script: install.sh
is_system_service: False
managed_services: klipper
info_tags:
desc=Beacon Surface Scanner</i></b> </li>
   ```

<li> Click Save, and Close Again</li>
<Li>Run the following Command (Or Power off/on your Giga Again - This is usually most effective)
<br /><b><i>systemctl restart klipper</b></i></Li> 
<li>From Fluidd, chose the Command Icon</li>
<li><i><b>Run: G28 X Y</i></b></li>
<li><i><b>Run: G0 X605 Y205 to center it on Bed 1</i></b> </li> 
<i><b><li>Run: BEACON_CALIBRATE</i></b></li>
<li>You must now run commands and lower your Z like your setting the Z offset with the Supplied Metal Feeler or a Piece of Paper
<br /> The Command is :<i><b>TESTZ Z=-0.01</i></b>  (Lower it as little or increase it to move it down faster until it barely touches the Feeler or Piece  of Paper
<li>Once you have your Z Set, Remove the paper/feeler, and Run Command:  <i><b>ACCEPT</i></b> (It should state calibrating Beacon)</li>
<li> <i><b>Run Command: SAVE_CONFIG</i></b></li>
<Li>Now Heat the Primary BED your using and Re-Run the Beacon_calibrate Process</Li>
<li> Once Completed and You have ran Save_Config your can run a bed Calibration.
<br /> Command:  <i><b>BED_MESH_CALIBRATE</i></b> </li>
<h3> You’re now ready to print.</H3>
</ul>

> [!TIP]
>On the first print, you’ll want to use babystepping via the GUI to fine adjust the first layer offset.
>After the print finishes, the offset can be automatically applied to the model with the <b>Z_OFFSET_APPLY_PROBE</b> command for future prints.
