# Giga-Beacon
<h2>This design requires the Beacon H - Normal Variant</h2>
 <i><b><H3>I am not responsible for any damage to your device, perform these modification at your own RISK!</H3></i></b>

https://github.com/user-attachments/assets/93804f33-2fc6-4fdc-bc21-9b3e94921d5a

<br />
<h2>Things to Consider</h2>
I highly recommend getting your Beacon from Lukes Lab so you can get the 5M USB for the Giga. (Make sure to notify them you want the Type H Normal Variant)
<br />https://www.lukeslabonline.com/products/beacon?variant=49920272597293
<br />I also highly recommend the standoffs for use with Beacon 
<br />https://www.lukeslabonline.com/collections/elegoo/products/elegoo-giga-standoffs-aluminum
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
***Before you do this, turn on your printaer and HOME ALL****
<ul>
<li>On your Klipper Screen, Click Settings > Advanced Settings > Root</li>
<li>Follow the Prompts to Root your Klipper and Allow SSH. Note the Username and PW</li>
<li>On your Pc Download Putty - https://www.putty.org/ </li>
<li>Open Putty and under Host Name type in the IP of your Giga. Should be on the right top corner of the Klipper Screen when connected to WIFI.</li>
<li>Click Connect, and log in with the supplied Credentials.   
<i><b><br />Username: Elegoo
<br />Password: giga3dp</li></i></b>
<li>Run these 3 commands 1 at a time, it will take some time to complete the Beacon Klipper Install.
<i><b><br />cd ~
<br />git clone https://github.com/beacon3d/beacon_klipper.git
<br />./beacon_klipper/install.sh</li>
<br /></i></b>
<li>Do not close putty, we will go back to this shortly</li>
<li>Navigate to Fluidd via your web browser using the IP from before</li>
<li>Choose the Configure Icon on the left, and click printer.cfg to started editing. (I recommend you backup this file to be safe before you begin)
<br /><img src ="https://github.com/user-attachments/assets/8ca86775-1220-49ff-ae42-e1b08427cf48"</img>
</li>
<li> Comment Out the PROBE Section Completely with #. It Should look similar to this:
<i><b><br /> #####################################################################
<br /># 	Probe
<br /> #####################################################################
<br />#[probe]
<br />#pin:^mcu1:gpio11
<br />#x_offset: -14.0
<br />#y_offset: -29.5
<br />#z_offset: 0.0
<br />#speed: 5.0
<br />#samples: 2
<br />#samples_result: median
<br />#sample_retract_dist: 3.0
<br />#samples_tolerance: 0.05
<br />#samples_tolerance_retries:4</li></i></b> 
<li>Add the following below the commented out Probe Section:
 <i><b><br />[beacon]
<br />serial: /dev/serial/by-id/usb-Beacon_Beacon_RevH_5BB811315157355957202020FF0E0F24-if00 #This will be REPLACED
<br />x_offset: -1 # update with offset from nozzle on your machine
<br />y_offset: -18 # update with offset from nozzle on your machine
<br />mesh_main_direction: x
<br />mesh_runs: 2</i></b>
<li>Open Up Putty and Run the following Command
 <i><b><br />ls /dev/serial/by-id</li></i></b>
<li>Copy the USB line that contains Beacon, mine for example was <i><b>"usb-Beacon_Beacon_RevH_5BB811315157355957202020FF0E0F24-if00"</b></i></li>
<li>Paste the USB info in the beacon section under Serial:(It Should look like above but <i><b>/dev/serial/by-id/YOUR COPIED USB DATA HERE)</b></i></li>
<li>Find the section [stepper_z]</li>
<li>Comment Out with #:
<i><b><br />#endstop_pin:PC11
<br />#position_endstop:0 </i></b>
<li>Add the Following lines at the end of stepper_z
 <i><b><br />endstop_pin: probe:z_virtual_endstop # use beacon as virtual endstop
<br />homing_retract_dist: 0 # beacon needs this to be set to 0</i></b>  </li>
<li>Under [stepper_z1] comment out with #:
<i><b><br /> #endstop_pin:PC10</i></b></li>
<li>Add Under [stepper_z1] :
<i><b><br />endstop_pin: probe:z_virtual_endstop # use beacon as virtual endstop</i></b></li>
<li>Find the Safe_z_home section and make it as follows:
<i><b><br />  [safe_z_home]
<br /> home_xy_position: 405,205
<br /> # speed: 150
<br />  z_hop: 3          
<br /> #z_hop_speed: 5</i></b></li>
<li> Click Save in the Top Right Corner, then close</li>
<li>Open moonraker.cfg</li>
<li>At the End Add the Following:
<i><b><br />[update_manager beacon]
<br />type: git_repo
<br />channel: dev
<br />path: ~/beacon_klipper
<br />origin: https://github.com/beacon3d/beacon_klipper.git
<br />env: ~/klippy-env/bin/python
<br />requirements: requirements.txt
<br />install_script: install.sh
<br />is_system_service: False
<br />managed_services: klipper
<br />info_tags:
<br />desc=Beacon Surface Scanner</i></b> </li>
<li> Click Save, and Close Again</li>
<Li>Run the following Command (Or Power off/on your Giga Again - This is usually most effective)
<br /> systemctl restart klipper</Li> 
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
</li>
<h3> This should now complete your configuration of the Beacon. Set your Z offset via for printing and start testing away.</H3>
</ul>
