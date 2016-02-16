[title]: - " fsurf (FreeSurfer Workflow)"
[TOC]
 
## Overview

[FreeSurfer](http://freesurfer.net/) is software package to analyze the MRI images of human brains. Here, we introdue the `fsurf` workflow that utlizes FreeSurfer.  `fsurf` is a complete pipeline for the brain image analysis on distributed high throughput 
computing platform. The advantages of using  `fsurf` are

     (1) No need to worry about the job schedular and write a job description file
     (2) Provides a complete pipeline to analyze the MRI image 
     (3) No need to worry about how to do the data transfers from/to the remote worker machines
     (4) Perfom analysis on multiple subjects via distributed high throughput computing

![fig 1](https://raw.githubusercontent.com/OSGConnect/tutorial-FreeSurfer/master/Figs/freesurfer_image_from_net.png )

## Tutorial files

It is easiest to start with the `tutorial` command. In the command prompt, type

     $ tutorial fsurf            # Copies input image file 

This will create a directory `tutorial-fsurf`. Inside the directory you will see the following file:

    test_defaced.mgz              # Input brain MRI file

Here, `test_defaced.mgz` is the brain MRI file. 

## Initial configuration

You need to set up the `fsurf` tool. Run on the command line:

     $ fsurf-config

This command may be placed at the end of your .bashrc file so that it will automatically run when you log in. 

## Process a Scan

A typical image analysis requires doing calculations on multiple-stages via autorecon1, autorecon2, and autorecon3.  All three steps are conveniently handled by `fsurf`. Now we do an analysis on `test_defaced.mgz`. In the file `test_defaced.mgz` the prefix `test` is the name of the subject which could be a number or name while the format  is `mgz` file format.


     $ fsurf  --submit --subject test --dir $PWD

Free surfer requires that the MRI file to be deidentified and defaced. The supplied `test_defaced.mgz` is already deidentified and defaced, so say `y` to the following questions. 

     Has the MRI data been deidentified (This is required) [y/n]? y
     Has the MRI data been defaced (This is recommended) [y/n]? y

After typing `y` to the above two questions, `fsurf` creates and submits the workflow 

     Creating and submitting workflow
     Workflow submitted with an id of 20160119T100055-0600

The id of your workflow is `20160119T100055-0600`. The id is useful for checking, removing and getting the output of the workflow. 


### List Workflows

Run the command below to get a list of ids for your workflows that are running or have already  completed:

     $ fsurf --list 
     Current workflows
     Subject    Workflow             Submit time          Cores Used      Status    
     test       20160119T100055-0600 10:00 01-19-2016     2               Running   


### Get output from a completed workflow

See the workflow is completed. In the event of completion, the staus of the workflow should be `Success` as follows

     $ fsurf --list 
     Current workflows
     Subject    Workflow             Submit time          Cores Used      Status    
     test       20160119T100055-0600 10:00 01-19-2016     2               Success   

Run the command below to get the output of the completed workflow `20160119T100055-0600`:
 
     $ fsurf --output --id 20160119T100055-0600

Depending on the computer resources available this will typically require several hours to complete.  The output will be saved as an archive in the current working directory: 20160119T100055-0600.tar.bz2 . You can extract all the files in the archive using: 

    $ tar -jxvf 20160119T100055-0600.tar.bz2
 
 Similarly, you get the output of any completed  workflow with id `WorkflowID` 
 
     $ fsurf --output -id WorkflowID
     $ tar -jxvf WorkflowID.tar.bz2

### Remove a running or completed workflow

Run on the command line to remove an existing workflow:
   
    $ fsurf --remove --id WorkflowID

For example, to remove a running worflow with an id `20160119T100055-0600`, type

    $ fsurf --remove --id 20160119T100055-0600
    Workflow 20160119T100055-0600 removed successfully
    Waiting for running jobs to be removed...
    Jobs removed, removing workflow directory

This will not effect the files you have fetched with fsurf --output --id WorkflowID.
(remove check)
## Getting Help 
For assistance or questions, please email the OSG User Support team  at [user-support@opensciencegrid.org](mailto:user-support@opensciencegrid.org) or visit the [help desk and community forums](http://support.opensciencegrid.org).