# Charmhigh CHM-T560P4 Pick and Place Deployment
**Montana Technological University | Electrical Engineering Department**  
**Author:** Miller DeMark

---

## Overview

When Montana Tech's Electrical Engineering Department acquired a Charmhigh CHM-T560P4 pick-and-place machine, it came with virtually no documentation on how to get it operational. Getting the machine up and running fell to me, and this document is the result of that process, covering everything from initial software setup to running a full PCB population job. The goal was to create a repeatable, documented workflow so that anyone in the department could operate the machine without starting from scratch.

---

## What You'll Need

Before starting, make sure you have the following ready:

- Your KiCAD PCB file (`.kicad_pcb`)
- KiCAD installed on your machine
- The Converter Tool folder (available on the department OneDrive)
- A USB thumb drive
- Your PCB with solder paste already applied
- All component stacks loaded and seated properly on the machine

> **Note:** When setting up the Converter Tool for the first time, open `settings.ini` and set the language variable to `1` for English. The Converter Tool ships on the USB Charmhigh provides with the machine.

---

## Software Workflow

### Step 1: KiCAD Setup

Open your `.kicad_pcb` file in the PCB Editor. Before exporting, align the bottom-left corner of your PCB with the outer top-left corner of the workspace, the area where both X and Y axis values are positive. Get it as close to the boundary as possible without crossing it. The machine can handle a small margin of error during calibration, but the closer the better.

### Step 2: Exporting the Position File

Once your PCB is positioned correctly, export the component placement file:

1. Click **File → Fabrication Outputs → Component Placement (.pos)**
2. Set **Format** to `CSV`
3. Set **Units** to `Millimeters`
4. Set **Files** to `Separate files for front, back`
5. Ensure only **Use drill/place file origin** is checked
6. Click **Generate Position File**

This CSV is what gets fed into the Converter Tool in the next step.

### Step 3: CSV Formatting

The Converter Tool requires the CSV to be in a specific format before it can read it correctly. A Python script is included in the department's Converter Tool folder that handles this formatting automatically from a KiCAD export. Run the script on your exported CSV before proceeding. Note that the script is built for KiCAD exports specifically. It will not work correctly with exports from other PCB programs.

### Step 4: Converter Tool

Open the Converter Tool and click **Open** in the top-left corner, then select your formatted CSV. If everything was done correctly, your components will populate the screen. From there:

1. Click **Convert** to begin the conversion process
2. On the splash screen, select **CHM-560P4** and **Use both heads**, then click **OK**
3. You will be shown a stack assignment screen auto-populated from your CSV. Verify that each component's stack ID matches its physical location on the machine. If any are off, select **Modify Stack ID** and enter the correct stack number.

Once everything looks correct, export the file as a `.dpv` and save it to your USB thumb drive.

---

## Machine Setup

### Loading the .dpv File

Plug your USB drive into the machine and navigate to the **Files** section. Select **File Import** and choose your `.dpv` file, then hit **Update**. If you have multiple files, use **Import All**.

After importing, wait a full 20 seconds before doing anything else. This is not optional. The files are easy to corrupt mid-import, and if that happens the machine will lose all the settings from the Converter Tool, leaving only basic stack data. The wait is short and worth it.

### Opening and Editing the File

Navigate to the **Run** tab, select your imported file, and hit **Edit**. This is where you verify that everything transferred correctly from the Converter Tool and configure the machine-specific settings.

### Nozzle Selection

Input your nozzle types in the appropriate fields. Each nozzle has its number stamped on its collar. For example, nozzles 503 through 506. Make sure the numbers entered match the nozzles physically installed on the machine heads.

### Calibration

Calibration tells the machine exactly where your PCB is located so placements are accurate.

1. Change the **Mark point** field to **EC**. The fields will update automatically. This is normal.
2. Select **Calibrate**. The machine will use the top-left, bottom-left, and bottom-right-most components as reference points.
3. Press the **+** button to begin. The machine will move its crosshairs to the first calibration point.
4. For each point, align the crosshair to the exact center of the component's footprint. Since the machine picks from center, precision here directly affects placement accuracy.
5. Repeat for all three calibration points, then hit **Back** and save the file.

### Batch Runs

To populate multiple PCBs in one run, use the **Batch** feature. Click **Add** in the bottom-right corner and select how many boards you want to run. For a grid layout, use the **Array** function. Without it, boards will be arranged in a single line. After setting up the batch, hit **Edit** and calibrate each board individually to its bottom-left corner.

---

## Running the Machine

When setup and calibration are complete, select **Load** and then **Run**. The machine will begin placing components.

If an error occurs, the machine will stop, beep, and display an error message on screen. Pay close attention to these messages. They are the primary troubleshooting tool the machine provides. Common issues to check if placement problems occur:

- **Parts not being picked up:** check feeder alignment and slow down the part grab speed in head settings
- **Parts dropping mid-run:** verify nozzle size is appropriate for the component
- **Misaligned placements:** re-run calibration, paying close attention to crosshair centering

---

## Project Notes

- This documentation was written from scratch as no usable English-language guide existed for this machine at the time of deployment.
- The workflow covers a top-side (front) population run. Back-side population follows the same process using the back CSV export from KiCAD.
- The Python formatting script and Converter Tool are stored on the department OneDrive and should remain there for future use.
- Full documentation, the Python script, and additional guides are available in the [repository](https://github.com/mdemark18/pick-and-place) as it is completed.
