# Liquid-Argon-Temperature-DAQ

# Overview

## Built and tested a multi-channel RTD temperature monitoring system designed to measure vertical thermal gradients within a liquid argon cryochamber.

### **Project Highlights**

- Designed and assembled a RTD sensor array capable of submersion in liquid argon (~-186°C).

- Integrated a Playing With Fusion FDQ-30002 data acquisition board.

- Implemented real-time temperature monitoring and CSV data logging.

- Tested sensor response and temperature gradients in liquid argon.

- Evaluated sensor alignment, measurement consistency, and system limitations through experimental testing.

### **Technologies Used + Skills: Arduino, C++, Python, DAQ Control Scripting, RTD Sensors, Data Acquisition, CSV Logging, Plotting, Hardware Construction (Soldering, Wiring), Experimental Testing, Data Analysis**

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

 ## 1. The Problem
 
Liquid argon particle detectors used in neutrino research are highly vulnerable to thermal shock. For the target 4-ft cryochamber, **a vertical temperature differential exceeding 20°C across the two ends of the detector chassis could cause catastrophic material failure.**

**To mitigate this risk, temperature gradients within the cryochamber must be monitored during the filling process.** The goal of this project was to design, build, and test a temperature Data Acquisition (DAQ) rig capable of mapping vertical temperature gradients in real time. **By measuring temperatures at multiple depths, researchers can monitor thermal equilibration and dynamically adjust chamber fill rates to prevent potentially damaging temperature differentials from developing across sensitive equipment.**

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 2. Design Considerations & Limitations

Design Considerations:
- The price to beat is about $1500, which would be the cost of buying a similar unit fully made.


Limitations:
- **The absolute accuracy of the measurements has not yet been independently verified through calibration** (such as the use of a reference thermometer). Currently, the rig uses the FDQ-30002's native fault-checking functions to flag for faults which could give some insight to when a measurement is completely awry, but not whether a measurement is accurate.
- However, **the primary objective of this prototype is to monitor relative temperature differences between sensor depths. Consequently, sensor-to-sensor consistency and repeatability are the most important parameters**. If the collected data should ever be used for more than a relative temperature gradient, this will need to be addressed, but **within the constraints of the current objectives, we do not need to emphasize measurement accuracy in our design.**

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 3. Hardware Design and Construction

I soldered two RTDs to a PCB designed to mount to a secure permanent housing rod (Figure 1.). By mounting two RTDs at the same depth, I can catch imprecise measurements when they occur by comparing the temperature of two parallel readings which should be approximately the same. 
 
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


**On-Site Reconfiguration:** A miscommunication lead me to believe I would be testing my unit in the 4-ft cryochamber. On-site, however, I learned we would be using a smaller ~1.5-ft unit which would only be filled with about 6" of liquid argon. As I had designed the physical hardware with the 4-ft chamber in mind, I'd mounted the RTDs to the scrap PCB about ~1-ft away from its tip (see Fig 5.), which prevented the RTDs from reaching the liquid.

Fortunately, since I had used a semi-permanent construction for the prototype, the sensor harness was easily inverted on-site prior to inserting the sensor array within the liquid for data collection (C.f. Figure 6 & Figure 7).

>FIG 7: On-Site Reconfiguration. Kapton Tape Was Used to Secure Cable to Array Mount and Prevent Stress on the PCB and Solder Joints
<img width="975" height="212" alt="image" src="https://github.com/user-attachments/assets/d83ff3b5-50d4-4987-8295-515ce35d04dd" />
<br>
<br>
<br>

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 4. Scripting

**Arduino Script:** The Arduino-based DAQ firmware included several safeguards and data-handling features:

- **SD Card Detection:** Checks for SD card availability during initialization and alerts the operator if no storage device is detected, preventing data logging from proceeding without available storage.
- **RTD Fault Monitoring:** Utilizes the DAQ board's fault-detection capabilities to identify sensor faults.
- **Periodic Data Logging:** Records averaged RTD measurements to CSV at 2-second intervals.
- **Temperature Averaging:** Calculates additional 5-second averages for temperature monitoring and plotting.
- **Multi-Channel Acquisition:** Supports simultaneous monitoring of four RTD channels.

**Python Script:** **A Python script was written to communicate with the Arduino script and generate a live plot of the data.**
- Initial testing of the script revealed good results, however, **the Python script not sufficiently debugged by the time of the initial test, so the 5-second average was instead printed to the console for testing purposes, and the 2-second averages were used as the plotting data post-test (See Sec. Testing & Results).**


-------------------------------------------------------------------------------------------------------------------------------------------------------------------



 ## 5. Testing & Results
 
Test Objectives:
- Verify hardware 
I started by placing the lowest sensor an inch above the liquid. I observed the output monitor and waited for the temperature at each depth to stop changing. The unit was then submerged at different depths relative to the start position. Each time the unit was moved, the time of movement was noted and the depth it was moved by was measured and recorded. This procedure was done repeatedly at various depths. The purpose of this is to not only test the success of the DAQ unit prototype, but to also log data about how quickly the RTDs respond to changes in depth. 
<br>
<br>
[▶ Click Here to Watch the Liquid Argon DAQ Test Demonstration (After Clicking Link, Click "View Raw" to Download MP4)](./DAQ_Test_Demonstration_Video.mp4)
<br>
<br>
<br>
The live readout (a sample of which is shown in Figure 8.) worked as expected. What was not considered was that the limit for the low temperature fault flag would of course need to be adjusted for liquid argon temperatures, so during the course of the test the fault checker read "Low RTD Threshold Met". Additionally, the RTDs were wired such that channels 0 and 2 were at equivalent depths, and 1 and 3 were at equivalent depths, making the readout a little difficult to follow as it was not intuitively obvious that these RTDs were at the same depth from the readout alone.


>FIG 8: Live Readout Snippet
<img width="487" height="535" alt="image" src="https://github.com/user-attachments/assets/01bb129f-3dcd-423c-a62f-7a0476d190e4" />
<br>
<br>
<br>
<br>
<br>
<br>
>FIG 9: Plot of First Test of Temperature DAQ in Liquid Argon. 
<img width="3000" height="1800" alt="temperature_plot" src="https://github.com/user-attachments/assets/d6667f38-c2e4-49e1-830a-8960e582a503" />
Looking at the plot of the prototype test (Figure 9.): 
- Plotting starts at room temperature and ends at roughly room temperature.
- Temperature change upon movement of the rig appears exponential
- The floor temperature of the liquid argon was ~-190°C.
- At ~1000s, the lower RTDs were fully submerged while the uppers remained outside the liquid.
- At ~1300s, the full unit was submerged, though the lower RTDs did not change in temperature, indicating the two depths we're approximately at equilibrium.

<br>
<br>
<br>

**Observed Variation in Temperature with Angle:** 
As mentioned in the Hardware and Test Setup section, two RTDs were mounted to each PCB, meaning they would be at approximately the same depth. By doing this, it was believed I could check for faults by initiating a flag whenever the temperature difference at the same depth was too great. 
- **As was discovered upon the initial test, however, the sensors are more sensitive to depth than initially thought, as the temperature differential of liquid argon can be rather large at comparably small differences in depth.**
- For instance, **tilting the unit only a few degrees at the head such that the two sensors were at a depth difference of only a few millimeters was observed to result in differences in temperature on the order of 3-5 degrees Celsius.**
- This observance was not pursued qualitatively, which was a mistake in retrospect (discussed more in sec "Discussion & Future Improvements").

**This means a method of ensuring the unit is aligned vertically may need to be considered in the next iterations**. 

>FIG 10: Up-Close Shot of Sensor Array Showing the Accumulated Frost from the Extreme Temperature Exposure
<img width="571" height="981" alt="image" src="https://github.com/user-attachments/assets/a2815d9a-b08e-4c80-99e4-a69ef96f6140" />
<br>
<br>
<br>

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 6. Cost Analysis

**Cost Effective** **The price to beat was ~$1500.** Looking at the component list for the proposed final design:
- DAQ Board: ~120 dollars
- 4 Shields (for 16 additional channels to get to 20 channels total): 4x$80 = $320
- DAQ & Shield Housing: ~$75
- Stackable Header Pins for Shields: ~$15
- 20 RTDs: ~$90.
- 10 Mount PCBs: ~$5.
- Cable: Since each cable only supports 4 channels, we'd need 5 cables for the final build, which at 6-ft would amount to ~$240
- Solder: ~$10
**Estimated Total: ~$875, meaning an approximately 42% estimated reduction in cost compared to a store-bought temp-sensing rig.**


-------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 7. Discussion & Future Improvements



**Discussion:** 
These results demonstrate that the prototype can detect temperature changes at different depths during liquid-argon exposure. 
- The lower sensors reached a stable reading while the upper sensors remained above the liquid, and the upper sensors subsequently reached a similar stable reading after submersion.
- This establishes the prototype's basic ability to monitor temperature at multiple depths.
- Further testing in the full-size cryochamber, particularly with detector equipment installed, would be necessary to validate its ability to monitor vertical thermal gradients under operational conditions.

**Problems with Current Prototype:**
- **Conductor Pair Shielding:** The conductor pair shielding was chosen to be left on the wires over the section where the cable's main jacket was removed. While the cable was specifically chosen to withstand liquid argon temperatures, exposure to extreme temperatures appeared to make the metal shielding slightly brittle (see Figure 10.), and over time it possible bits of this shielding will break off.
- **Non-Intuitive RTD Naming:** The RTDs were wired such that RTD0 and RTD2 were at equivalent depths and RTD1 and RTD3 were at equivalent depths. The live readout, which listed the RTDs in numerical order, consequently had an awkward sequencing (see Figure 8.).

**Ideas for Future Iterations/Solutions to Problems**
- **Conductor Pair Shielding:** Specialized insulation may be shrunk over the individual shielded conductor pairs to protect the shielding. Alternatively, the shielding may be cut off entirely at the point where the jacket is stripped, although consequences on potential resulting interference will need to be considered.
- **RTD Channel Rewiring/Renaming:** Channels should be rewired so RTDs at parallel depths are numerically next to each other (e.g. RTD0 and RTD1 should be at same depth). Alternatively, these channels can be renamed in the script, although for wiring purposes it will be more intuitive if the RTD channels are wired physically next to each other, so rewiring is likely the better choice.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

 ## Conclusion & Lessons Learned


 
The prototype type test was highly successful. It revealed much about what works with the first prototype and what needs to be changed. With the data I collected, the goals for the next iteration have been mapped out clearly. Namely:
- Ensuring rigid vertical alignment to prevent misreadings of the temperature at the desired depths
- Update the readout script so RTDs at equivalent depths are readout next to each other, lowering Low RTD Threshold flag temp to account for cryogenic temps
- Addition of sensors to more than two depths
- Completion of the python live-readout script
- Permanent mounting to 4-ft housing + soldering of wires to DAQ board

**Lessons Learned:**
- **Documentation:** Documentation is a quintessential skill as an engineer, as it allows for greater analysis of results, leads to more thorough consideration of design and iteration, and serves as the fundamental form of book-keeping to track projects organic projects which evolve through time. **This project marks a great step-up in my own documentation skill, as I routinely took pictures, uploaded rough drafts of my scripts to Github, and wrote down accompying notes for these things. I also wrote down my design ideas, thought processes, project goals, and questions for the physicists I was designing the unit for and notes about what they told me.** **However, there was plenty I still missed. For instance, no pictures exist of the "No SD detected" flag or fault detection demonstrations discussed earlier (see sec. 4 "Scripting").**
- **Quantitative Measurement:** Upon reflection, a stronger emphasis on quantitative measurement needs to be deployed on future projects. For instance, when it was observed that varying the angle of the rig by only a few degrees resulted in a deviation of temperature of RTDs mounted to the same depth on the rig on the order of 3-5°C, I should have done more than just note this observation but used a protractor to measure the phenomena explicitly.
- **Thinking Ahead:** Certain things cannot be reasonably predicted without testing. With hindsight, it is easy to see the mistakes, such as not performing quantitative testing for DAQ rig's response to deviations in angle from perfect vertical alignment, but nothing reveals these mistakes as quickly and blatantly as testing or simulation. **However, with each project, predicting what's most likely going to be a point of failure or be overlooked beforehand becomes even easier.**
-**Communication and Design Constraints:** Communication with the physicists who asked me to build the sensor was of utmost importance. They laid the groundwork for the essential design constraints and considerations. Without frequent communication, a project is at risk of over or under-engineering, going out of budget, or just plain not capable of achieving its fundamental requirements.

