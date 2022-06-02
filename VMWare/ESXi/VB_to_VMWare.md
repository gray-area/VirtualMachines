# Error: Missing Child Element VirtualHardwareSection / This OVF package requires unsupported hardware

## The problem:

In order to get the .OVF file to work on ESXi, the file must first be converted, then edited, then imported. Each step is outlined below.

## Converting OVA to OVF

The first thing that you will need is the OFT tool from VMWare. This can be downloaded HERE.

Once the tool has been downloaded and installed, you will need to open a CMD prompt window in that directory. 

``cd "C:\Program Files\VMware\VMware OVF Tool" ``

From here, we can convert the file:

`` ovftool.exe -lax path\to\source.ova path\to\output.ovf ``

You will need to change the .ova file location to be where you currently have your .ova and your destination to where you would like to create the .ovf, .mf and .VMDK.

Once this completes, you will see the 3 new files that are in the directory you specified. 

## Editing the OVF file

You will need to open the .ovf file with a text editor in order to make the needed changes for ESXi to recognize the file. 

The first line you are looking for should look like this:

``<vssd:VirtualSystemType>virtualbox-2.2</vssd:VirtualSystemType>``

You will need to replace the ``virtualbox-2.2`` with ``vmx-07`` as shown below.

``<vssd:VirtualSystemType>vmx-07</vssd:VirtualSystemType>``

## Importing the new files

In order to import the new machine with the changes, you will need to:

open ``ESXi GUI > Virtual Machines > Import/Export > Import OVA or OVF``

It will ask you to give the VM a name. Give it a name and click the "Add Files" box on the page. 

You will need to add both the ``.OVF`` and the ``.VMDK`` files, then click next.

If you see a warning banner at the top of the Import screen, you will need to edi the OVF file to remove the section that is causing an error.

To remove a section from the .OVF file, look for the ``<Item>`` and the ``</Item>`` tags for the section that is giving an error. Once located, delete everything in between these tags.

Save the file, then click back, until you are at the upload section. Re-add the ``.OVF`` and the ``.VMDK`` files again. Repeat this until no error is given. 
