# Giga-Beacon
<H3>I am not responsible for any damage to your device, perform these modification at your own RISK!</H3>
<br />I highly recommed getting your Beacon from Lukes Lab so you can get the 5M USB for the Giga. 
https://www.lukeslabonline.com/products/beacon?variant=49920272597293

<h2>Things to Print</h2>
Download & Print Giga Beacon Adapter.stl in ABS(Recommend)
You will need 6 m3x4 Heat Inserts. The LED Relocation side mount are inserted from the bottom
<ul>
<li>.16 line height</li>
<li>3 Walls</li>
<li>4 Top/Bottom Layers</li>
</ul>

<h2>Hot End Modifications</h2>
<ul>
<li>Remove the Hot End from the Printer(4 M3 Screws, 2 front, 2 back)</li>
<li>Remove the Case by 2 M3 Screws attaching it</li>
<li>Remove the Pobe, 2 screws attach it to the metal casing, then trace the wire to the board and fish the wire out of the casing</li>
 <li>Remove the 2 LED Screws and pull it to the side</li>
 <li>Use a Dremel with Cut off Wheel to cut the metal casing as shows in the RED Outlines in the picture <p><img src ="https://github.com/jranger615/Giga-Beacon/blob/main/photos/Hot-End%20Modification.jpg?raw=true"/></p>
 <p><img src ="https://github.com/jranger615/Giga-Beacon/blob/main/photos/Hot%20End%20Mod%20Completed.jpg?raw=true"/></p></li>
 <li>Using the cut off wheel Cut the Plastic Cover as shown in the picture
 <p><img src ="https://github.com/jranger615/Giga-Beacon/blob/main/photos/Plastic%20Cover%20Mod.jpg?raw=true"/></p></li>
 <li>Cut a hole in the top of the Plastic Casing big enough to fit the USB Cable End through, it can also be snaked through near the fan.</li>
 <li>Run the USB through the Cable Chain to the hot end location, fish it through the plastic cover, and down where the LED Use to be</li>
 <li>Mount the USB on the Adapter with 2 M3 Screw</li>
 <li>Mount the Adapter to the stock Sensor Location using 2 M3 Screw, make sure it is tighter fitting again the side metal case near the LED. This helps make sure its flat</li>
 <Li>Attach the USB to the Beacon, and secure the beacon to the Adapter with 2 M3 Screw
  <p><img src ="https://github.com/jranger615/Giga-Beacon/blob/main/photos/Beacon-Mounted.jpg?raw=true"/>
 <img src ="https://github.com/jranger615/Giga-Beacon/blob/main/photos/Beacon%20Mounted-Front.jpg?raw=true"/></p>
 </Li>
 <li>ReMount your Hot End to the Printer</ki>
</ul>
 
<h2>Beacon Install/Configuration File Modifications</h2>
***Before you do this, make sure you have your printer turned on and HOME ALL****
<ul>
<li>On your Klipper Screen, Click Settings > Advanced Settings > Root</li>
<li>Follow the Prompts to Root your Klipper and Allow SSH. Note the Username and PW</li>
<li>On your Pc Download Putty - https://www.putty.org/ </li>
<li>Open Putty and under Host Name type in the IP of your Giga. Should be on the right top corner of the Klipper Screen when connected to WIFI.</li>
<li>Click Connect, and log in with the supplied Credentials.   
<br />Username: Elegoo
<br />Password: giga3dp</li>
<li>Run these 3 commands 1 at a time, it will take some time to complete the Beacon Klipper Install.
<br />cd ~
<br />git clone https://github.com/beacon3d/beacon_klipper.git
<br />./beacon_klipper/install.sh</li>
<br />
 <li>Do not close putty, we will go back to this shortly</li>
<li>Navigate to FLuidd via your web browser using the IP from before</li>
<li>Choose the Configure Icon on the left, and click printer.cfg to started editing. (I recommend you backup this file to be safe before you begin)</li>
 <li> Comment Out the PROBE Section Compeltly with #. It Should look similar to this:
<br /> #####################################################################
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
<br />#samples_tolerance_retries:4</li>
<li>Add the following below the commented out Probe Section:
<br />[beacon]
<br />serial: /dev/serial/by-id/usb-Beacon_Beacon_RevH_5BB811315157355957202020FF0E0F24-if00 #This will be REPLACED
<br />x_offset: -1 # update with offset from nozzle on your machine
<br />y_offset: -18 # update with offset from nozzle on your machine
<br />mesh_main_direction: x
<br />mesh_runs: 2
<li>Open Up Putty and Run the following Command
<br />ls /dev/serial/by-id</li>
<li>Copy the USB line that contains Beacon, mine for example was "usb-Beacon_Beacon_RevH_5BB811315157355957202020FF0E0F24-if00"</li>
<li>Paste the USB info in the beacon section under Serial:(It Should look like /dev/serial/by-id/YOUR COPIED USB DATA HERE)</li>
<li>Find the section [stepper_z]</li>
<li>Comment Out with #:
<br />#endstop_pin:PC11
<br />#position_endstop:0 
<li>Add the Following at the end of stepper_z
<br />endstop_pin: probe:z_virtual_endstop # use beacon as virtual endstop
<br />homing_retract_dist: 0 # beacon needs this to be set to 0  </li>
 <li>Under [stepper_z1] comment out with #:
<br /> #endstop_pin:PC10
 <br />Add:
 <br />endstop_pin: probe:z_virtual_endstop # use beacon as virtual endstop</li>
 <li>Find the Safe_z_home section and make it as follows:
 <br />  [safe_z_home]
 <br /> home_xy_position: 405,205
 <br /> # speed: 150
 <br />  z_hop: 3          
 <br /> #z_hop_speed: 5</li>
 <li> Click Save in the Top Right Corner, then close</li>
 <li>Open moonraker.cfg</li>
 <li>At the End Add the Following:
<br />[update_manager beacon]
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
<br />desc=Beacon Surface Scanner </li>
<li> Click Save, and Close Again></li>
<Li>Run the following Command (Or Power off/on oyur Giga Again - This is usually most effective)
<br /> systemctl restart klipper</Li> 
 
</ul>
