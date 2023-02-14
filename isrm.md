---
layout: page
title: Running the ISRM Tool on Mac
permalink: /isrm_tool/
main_nav: false
---
<p> <em>Last Updated: February 14, 2023 </em> </p>


This web page provides a detailed description of how to run the ISRM Tool, built as a collaboration with UC Berkeley, University of Washington, and California's Office of Environmental Health Hazard Assessment. This document provides a full write-up of how to run this code pipeline on Mac OS. For instructions on how to run the tool in the Google Cloud, please see the instructions [here](https://docs.google.com/document/d/1aurYIaGMi6BCvQaK6cEyrb5amSAX8TXTYiB2ko2N8FU). The [Github repository](https://github.com/lkoolik/isrm_health_calculations/) has more information about the code details.

---

# Background #
The tool is a repository of scripts used for converting emissions to concentrations and health impacts using the InMAP Source Receptor Matrix (ISRM). This first working version of the tool is designed around the California ISRM, however it is possible to run other ISRMs as long as they are in the correct format. For a detailed description of the code repository and to download the code, visit the Github repository [here](https://github.com/lkoolik/isrm_health_calculations#readme). The following two sections of this page are reproduced from the Github repository.

## Purpose and Goals ##
The Intervention Model for Air Pollution (InMAP) is a powerful first step towards lowering key technical barriers by making simplifying assumptions that allow for streamlined predictions of PM<sub>2.5</sub> concentrations resulting from emissions-related policies or interventions.\[[1](https://doi.org/10.1371/journal.pone.0176131)\] InMAP performance has been validated against observational data and WRF-Chem, and has been used to perform source attribution and exposure disparity analyses.\[[2](https://doi.org/10.1126/sciadv.abf4491), [3](https://doi.org/10.1073/pnas.1816102116)\] The InMAP Source-Receptor Matrix (ISRM) was developed by running the full InMAP model tens of thousands of times to understand how a unit perturbation of emissions from each grid cell affects concentrations across the grid. However, both InMAP and the ISRM require considerable computational and math proficiency to run and an understanding of various atmospheric science principles to interpret. Furthermore, estimating health impacts requires additional knowledge and calculations beyond InMAP. Thus, a need arises for a standalone and user-friendly process for comparing air quality health disparities associated with various climate change policy scenarios.

The ultimate goal of this repository is to create a pipeline for estimating disparities in health impacts associated with incremental changes in emissions. Annual average PM<sub>2.5</sub> concentrations are estimated using the [InMAP Source Receptor Matrix](https://www.pnas.org/doi/full/10.1073/pnas.1816102116) for California.

## Methodology ##
The ISRM Health Calculation model works by a series of two modules. First, the model estimates annual average change in PM<sub>2.5</sub> concentrations as part of the **Concentration Module**. Second, the excess mortality resulting from the concentration change is calculated in the **Health Module**. More details are included in the Github Repository [here](https://github.com/lkoolik/isrm_health_calculations#readme).

---

# Setting Up on Mac #

The tool was developed on MacOS Monterey on the Apple M1 with 16 GB Memory. The instructions below may need to be adapted for different processing capabilities. These instructions assume a base level of comfort navigating your file directories in the terminal. For more information on commands you need for following these instructions, see [this article](https://www.makeuseof.com/tag/mac-terminal-commands-cheat-sheet/) on basic Mac OS commands.

## Setting Up Python ##

**Install Python**. In order to run the ISRM Tool, the computer must have Python installed. I recommend following the guidance provided by [Anaconda](https://docs.anaconda.com/anaconda/install/mac-os/) for downloading Python on Mac. It is also recommended that you install Anaconda Navigator for ease setting up a virtual environment.

**Virtual Environment**. The next step is to create a virtual environment for storing the proper versions of libraries required to run this code pipeline. Details on the specific requirements for the ISRM Tool are specified in the Github Repository's [requirements.txt](https://github.com/lkoolik/isrm_health_calculations/blob/main/requirements.txt) file. To set this up, it is recommended that you download this text file and save it on your computer. There are two ways you can set up your virtual environment.

1. **Option 1: Anaconda GUI**. Within the Anaconda Navigoator, select the tab "Environments" on the left-hand side. At the bottom, select "Import" to create a new environment from a requirements file. Download the requirements.txt file from the Github repository, and import this file (note: you may need to manually switch your import GUI to search for "Pip requirement files" instead of "Conda environment files"). Set your virtual environment name to "isrm_calcs_env" to be consistent with the rest of this guide.

2. **Option 2: Terminal**. Navigate to your directory of choice using `cd [directory]` in your Terminal. Follow the instructions from the official Python documentation [here](https://docs.python.org/3/tutorial/venv.html) to create your new virtual environment. Set your virtual environment name to "isrm_calcs_env" to be consistent with the rest of this guide. Next, activate that environment by running `source isrm_calcs_env/bin/activate`. Once the environment is created and activated, import the requirements document by running `python -m pip install -r requirements.txt`. 

You will test that your virtual environment is set up properly in the next section. If you find that it was not set up properly, you can manually update or install the missing libraries/packages using `pip` or the Anaconda Navigator GUI.

## Clone Repository ##

It is highly recommended that you clone the repository to keep your code up to date. If you create a static copy, you may miss future changes to the code. You may consider creating a free Github account ([instructions](https://docs.github.com/en/get-started/signing-up-for-github/signing-up-for-a-new-github-account)) and set up your computer to connect with your account ([instructions](https://docs.github.com/en/get-started/getting-started-with-git)). However, it is not required to have your own Github account.

Navigate to the Github repository [here](https://github.com/lkoolik/isrm_health_calculations). To clone the repository:

1. Navigate to the directory where you want the code to be saved from within your Mac Terminal: `cd [path/for/code]`

2. On the Github interface, click the green "Code" button and copy the https url.

3. In your Terminal, type: `git clone [url]`

For consistency with this tutorial, name the parent directory "isrm_health_calculations". Your directory should match the screenshot below.
 
![Screenshot of the directory once the repository is cloned](/assets/isrm_tutorial/directory_setup_before_data.png)


## Download Data ##

Within the `isrm_health_calculations` folder, create a new folder called "data". Download the data stored in [this Google drive](https://drive.google.com/drive/folders/14j-yB43YgZgPSPvhjxjdEbhLe6tR0SEc?usp=sharing) into that folder. Note: you should preserve the structure sub-directory "CA_ISRM" if you intend to use the California ISRM. If you have a different ISRM file, mimic this structure with your ISRM file.

## Test Code ##

Once you have everything ready to go above, your directory should mirror the screenshot below.

![Screenshot of the directory when ready to run](/assets/isrm_tutorial/directory_when_ready.png)

Now, you are ready to test the code. We will do this in two steps.

1. **Confirm Python Works**. In the terminal, navigate to the directory where this code is saved. Type the following command, which should return the built-in help statement for the ISRM Tool:

 `python isrm_calcs.py -h`

 This is successful if you get the following help message returned (ignore the colors):

    usage: isrm_calcs.py [-h] [-i INPUTS]

    Runs the ISRM-based tool for estimating PM2.5 concentrations and associated health impacts.

    optional arguments:
      -h, --help            show this help message and exit
      -i INPUTS, --inputs INPUTS
                            control file path

2. **Create a Test File**. 

 Within the "templates" folder of the isrm_health_calculations directory, there should be a text file called "control_file_template.txt". Copy this file to a directory of choice. For the purposes of this exercise, I will make a copy called "control_file_test.txt".
 
 Open the text file with a text editor (e.g., TextEdit, Notepad++). Make the following changes. Note - in a future section, I will discuss how to update this control file.
 
        ╓─────────────────────────────────╖
        ║  HEALTH RUN CONTROLS            ║
        ║  These should be set to Y or N  ║
        ╙─────────────────────────────────╜
        - RUN_HEALTH: N
        - RACE_STRATIFIED_INCIDENCE: N
        
        ╓────────────────────────────────╖
        ║ RUN CONTROLS                   ║
        ║ These should be set to Y or N  ║
        ╙────────────────────────────────╜
        - CHECK_INPUTS: Y
        - VERBOSE: Y
        
        ╓──────────────────╖
        ║  OUTPUT OPTIONS  ║
        ╙──────────────────╜
        - REGION_OF_INTEREST: 
        - REGION_CATEGORY: 
        - OUTPUT_RESOLUTION: 
        - OUTPUT_EXPOSURE: N
    
 Back in the terminal in the isrm_health_calcs directory with the isrm_calcs_env virtual environment activated, type the following prompt:

   `python isrm_calcs.py -i 'path/to/control/file/control_file_test.txt'`
   
   If this worked properly, you should get a box pop up with the name of the tool and the version. Then, you should get a number of bulleted messages in simple English about problems running the code. An example is below. These messages are okay and mean that Python is set up properly! 
   
   `* Issue finding ISRM_NH3.npy in the provided ISRM directory`
   `<< ERROR: Control file was successfully imported but inputs are not correct >>`

---

# Running on Mac #

The next section will describe how to run ISRM calculations on your Mac provided you have an emissions file. If you want to follow along step-by-step with this guide, feel free to download my [sample data](https://drive.google.com/drive/folders/1V0H_JLPpnWvqUfcADPi0JI_kd71Shbxm). The sample data is the California EMFAC model calendar year 2000 dataset.

## Setting Up Emissions File ##

In order to run the ISRM Tool, you will need to provide it with an emissions input file as either a shapefile or feather file. Shapefiles can be created using ArcGIS, QGIS, or coding languages like Python or R. Feather files are best created in Python.

The emissions file needs to have the following columns in order to run properly. Column names are bolded with descriptions following.
* **I_CELL**: ID column, just needs to be unique
* **J_CELL**: ID column, just needs to be unique
* Five emissions columns. These can have any units of mass per time, so long as they are all the same.
   * **PM25**
   * **NH3**
   * **VOC**
   * **NOX**
   * **SOX**
* **HEIGHT_M**: source release height in meters. This can be slightly imprecise, since things are binned into the three layers of the ISRM (0-57 m, 57-140 m, > 760 m)

Save this file in a directory of your choice, but be sure to write this directory down in your notes.

## Setting Up Control File ##

The control file is the central input file for directing your tool run. 

1. **Make a copy of the control file.** Within the directory where the tool is saved, find the "templates" folder, and copy the "control_file_template.txt" to a directory of your choice. Note this directory.
2. **Edit the control file.** Open the copy of the control file in a text editor (e.g., TextEdit). A description of each field is below.

 <table cellspacing="0" cellpadding="0">
  <tr>
    <th>Input</th><th>Required?</th><th>Description</th>
  </tr>
  <tr>
    <td>Division 1</td><td>Division 2</td><td>Division 3</td>
  </tr>
  <tr class="even">
    <td>Division 1</td><td>Division 2</td><td>Division 3</td>
  </tr>
  <tr>
    <td>Division 1</td><td>Division 2</td><td>Division 3</td>
  </tr>
  <tr>
    <td>Division 1</td><td>Division 2</td><td>Division 3</td>
  </tr>
  <tr>
    <td>Division 1</td><td>Division 2</td><td>Division 3</td>
  </tr>
