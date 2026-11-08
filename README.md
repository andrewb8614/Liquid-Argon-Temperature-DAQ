# Liquid-Argon-Temperature-DAQ

## Overview
 Built and tested a submersible temperature data-acquisition (DAQ) rig to capture and log real-time temperature data at various depths within a liquid argon chamber (-186°C). Collected data (Resistance, Temperature, and Fault Status) for each RTD is read out every half second to monitor. The data for each RTD is averaged every 2 seconds and written to a CSV file for documentation purposes. The data for each RTD was additionally averaged every 5 seconds with the intention of plotting. 
---------------------------------------------------------------------------------------------------------------------
## 1. Hardware & Test Setup

 Two RTDs were mounted to a PCB designed to mount to a secure permanent housing rod (Figure 1.). By mounting two RTDs at the same depth, I can catch imprecise measurements when they occur by comparing the temperature of two parallel readings which should be approximately the same. 
 
>FIG 1: Up-Close Shot of RTD’s Soldered to the Depth Mount Boards
<img width="558" height="709" alt="image" src="https://github.com/user-attachments/assets/7b36319f-bee9-4d88-9bc1-321002127bca" />
<br>
<br>
<br>

The Playing with Fusion FDQ-30002 RTD Data Acquisition board (Figure 2.) was chosen as the DAQ unit because of its Arduino compatible core, ability to add additional RTD channels through the use of shields, and native fault-checking, live data monitoring, and data logging functions.
  
>FIG 2: Playing with Fusion FDQ-30002. Four Channels Are Supported Without the Use of Shields, as Pictured. For Testing, the Channel Wires are Connected to the Terminals Via Spring Clamps. For a More Permanent Build, the Terminals Can Be Connected to Via SM Soldering. Each Channel Was Wired in a Two Wire Configuration.
<img width="4080" height="3060" alt="IMG_20261016_162845174" src="https://github.com/user-attachments/assets/aeec50e9-0373-4647-92da-e8da611731c4" />
<br>
<br>
<br>


 A single PCB mount containing 2 RTDs was initially tested using a 1-ft strip of scrap cable (Figure 3.). The blue cable is an 8 conductor cable of solid strand, meaning I can route 4 RTDs or two PCBs for each cable I use. Because it had already been vetted for cryogenic liquid temperatures, and because there was a great abundance of it on-site, it was the natural choice for wiring.
>FIG 3: Original Test Setup using 1-ft of Scrap cable
<img width="975" height="259" alt="image" src="https://github.com/user-attachments/assets/40078acb-0269-4bf0-ac29-372c977bf785" />
<br>
<br>
<br>

 After successful tests of wiring with the blue cable at room temperature, the 1-ft cable was replaced by a 6-ft cable for remote collection (Figure 4.).
>FIG 4: Sensor and DAQ Rig Expanded to 6-ft Cable Length  
<img width="975" height="658" alt="image" src="https://github.com/user-attachments/assets/b3a52648-7b16-4a07-9d5b-583df1ffe2fc" />
<br>
<br>
<br>
 
>FIG 5: RTD Array Attached to Mount. Stainless Steel Machine Screws and a Scrap PCB Originally Designed for Cryogenic Temperatures were Used for Initial Testing. Later Iterations Planned to Involved a Specialized Mount of Longer Length (~4ft)
<img width="928" height="391" alt="image" src="https://github.com/user-attachments/assets/9c139318-f78a-4d1b-a95c-5484241eb615" />
<br>
<br>
<br>

>FIG 6: Up-Close View of Figure 
<img width="889" height="319" alt="image" src="https://github.com/user-attachments/assets/49e79c7c-e5e9-4dc9-994b-c9d46744012f" />
<br>
<br>
<br>


**On-Site Reconfiguration:** During testing, fluid availability limited the liquid argon depth to ~6 inches. The sensor harness was then inverted on-site prior to inserting the sensor array within the liquid for data collection (C.f. Figure 6 & Figure 7).

>FIG 7: On-Site Reconfiguration. Kapton Tape Was Used to Secure Cable to Array Mount and Prevent Stress on the PCB and Solder Joints
<img width="975" height="212" alt="image" src="https://github.com/user-attachments/assets/d83ff3b5-50d4-4987-8295-515ce35d04dd" />
<br>
<br>
<br>

 ## 3. Data Logging & Results
I started by placing the lowest sensor an inch above the liquid. I observed the output monitor and waited for the temperature at each depth to stop changing. The unit was then submerged at different depths relative to the start position. Each time the unit was moved, the time of movement was noted and the depth it was moved by was measured and recorded. This procedure was done repeatedly at various depths. The purpose of this is to not only test the success of the DAQ unit prototype, but to also log data about how quickly the RTDs respond to changes in depth. 

<br>
<br>
<br>
The live readout (a sample of which is shown in Figure 8.) worked as expected. What was not considered was that the limit for the low temperature fault flag would of course need to be adjusted for liquid argon temperatures, so during the course of the test the fault checker read 
FIG 8: Live Readout Snippet
<img width="487" height="535" alt="image" src="https://github.com/user-attachments/assets/01bb129f-3dcd-423c-a62f-7a0476d190e4" />
The python plot was not sufficiently debugged by the time of the initial test, so the 5-second average was instead printed to the console for testing purposes.

 As mentioned in the Hardware and Test Setup section, two RTDs were mounted to each PCB, meaning they would be at approximately the same depth. By doing this, it was believed I could check for faults by initiating a flag whenever the temperature difference at the same depth was too great. As was discovered upon the initial test, however, the sensors are more sensitive to depth than initially thought, as the temperature differential of liquid argon can be rather large at comparably small differences in depth. For instance, tilting the unit only a few degrees at the head such that the two sensors were at a depth difference of only a few millimeters was observed to result in differences in temperature on the order of 3-5 degrees Celsius. This means the data will need to be handled carefully to be accurately interpreted, and a means of ensuring the unit is aligned vertically may need to be considered in the next iterations. 

>FIG 8: Up-Close Shot of Sensor Array Showing the Accumulated Frost from the Extreme Temperature Exposure
<img width="571" height="981" alt="image" src="https://github.com/user-attachments/assets/a2815d9a-b08e-4c80-99e4-a69ef96f6140" />
<br>
<br>
<br>
 


[▶ Click Here to Watch the Liquid Argon DAQ Test Demonstration (After Clicking Link, Click "View Raw" to Download MP4)](./DAQ_Test_Demonstration_Video.mp4)
