

##### Beaglebone
````bash
# Default login user
user: debian
pw:   temppwd

## Powering on/off
# Boot linux from the eMMC
Remove SD card and boot

# Boot linux from the SD card
Add SD card, hold USER button down and apply power

# Flash eMMC with SD card
Insert SD card
Hold USER button and connect power
Hold for 5-7seconds

# User LEDs
- USR0 is configured at boot to blink in a heartbeat pattern
- USR1 is configured at boot to light during microSD card accesses
- USR2 is configured at boot to light during CPU activity
- USR3 is configured at boot to light during eMMC accesses
USR0 is on the edge of the board


# Setup beaglebone
http://beagleboard.org/getting-started


# Resolve error 'the current language is not supported by the device driver installation wizard beaglebone'
when trying to install beaglebone drivers

1. Download and setup 7zip
2. Right click BONE_D64.exe -> 7zip -> Extract to BONE_D64\
3. Edit dpinst.xml
4. Add below first <\language> tag:
  <!-- English (Finland) -->
  <language code="0x1000"></language>
5. Run dpinst.exe

This adds the current OS language to the supported languages list.
See all the language codes here:
https://www.ryadel.com/en/microsoft-windows-lcid-list-decimal-and-hex-all-locale-codes-ids/
````



##### EAGLE notes
````bash
#### Control panel ####

# Take custom librarys into use
Create custom_libraries folder under EAGLE projects dir and add .lbr file under it, for example:
OSD3358_BAS_RefDesignParts.lbr
Also in the control panel click the gray dot next to the library name, it should change to green


#### General board layout notes ####
# Routing high speed traces
For example USB data lines should be the same length, or max. 1% difference in trace lengths. Measure the trace
lengths and if the other one is too short, a length matching bends can be created to the end of the trace. Width
betweend the bend should be greater than 3x the trace width and the angles should be at least 135 degrees.


#### Schematic view ####

# Create bill of material
Select schematic view -> ULP -> bom.ulp Bill Of Material -> Save...

# Things to check before sending gerber files to manufacturer
- Run ERC check ( Electrical Rule Check )
- Make sure the components are available
- Check that every label connects to the right place
- Check that components have values.
- Add comments and circuit names


#### Board layout ####

# Creating Gerber files
Layout view -> File -> CAM Processor -> Load Job File (next to save disk-image) -> Local CAM Jobs -> examples
-> Third Party -> OSH Park -> OSH Park 4 layer -> Check that everything is OK -> Process Job -> Save

# Force routing through pins
Select route airwave tool and select 'Ignore obstacles' from the top toolbar

# Measure trace lengths. ( Other information also available here, for example max current based on trace length )
Paste this command in the command line:
run length-freq-ri

# Skip thermal gaps when doing copper pour
Select 'Thermals off' from the polygon menu in the top toolbar after selecting polygon tool. This needs to be selected
before doing the polygon.

# Create board borders ( dimensions ).
Ensure that the board starts from the white dottex cross mark on the view.
Click dimensions tool and go through the edges of the board. Make sure that outside of the board is gray colored and
the board itself is black. Check from Manufacturing previw windows that the board layout is ok.

# Create ground and power planes ( layers )
Ensure that there is enough layers to do this. With bottom and top signal layers already used, more layers is needed.
Then create a copper pour over the whole board

# Add more layers
DRC -> Layers -> dropdown 2 - 16 layers.

# Connect via's to specific layer
Right click via and set the name. For examle to place via's to ground set the name to GND, or to power source, change the name
to for example SYS_3V3

# Add copper pours
Click polygon in the toolbar and create a full loop with it to create the borders of the pour, then click ratsnest to pour the copper.

# Remove golden circle around via when previewing
DRC -> Masks -> Limit. Set to for example 25mil.

# Switch layers while routing
Click Scroll wheel and select another layer to continue on another layer, this will place a via.

# Check for board layout errors
DRC -> check

# Good font size for component value on silkscreen:
32 with 8% ratio

# Things to check before sending gerber files to board manufacturer
- All the component designators are placed appropriately next to their corresponding components.
- Pin 1 is marked for necessary components. This includes all ICs, connectors and polarized capacitors.
- There is no silkscreen over any component pads. This can lead to bad solder joints.
- The font size of the component designators is as large as possible given the space constraints of the board so that the text is readable.
- Try to not place silkscreen over vias. Silkscreen has a hard time adhering to the annular ring of a via which can make the text difficult to read.
````

