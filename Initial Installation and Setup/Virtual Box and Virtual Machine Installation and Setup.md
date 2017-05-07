# Virtual Box and Virtual Machine Installation and Setup

## Oracle Virtual Box
<OL>
<LI>The latest version of Oracle Virtual Box for running a Virtual Machine (VM) on your local PC can be downloaded from this web site - http://http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html.

![image](https://cloud.githubusercontent.com/assets/19809692/25776600/a16960fc-3290-11e7-95ca-b6545df15eaa.png)

</LI>
<LI>Choose an installer or package file applicable to your Operating System.</LI>
<LI>Once the appropriate file from the above web site is downloaded, uncompress and run the executable application included in it to install the Virtual Box.</LI>
<LI>Run the Virtual Box post installation. It will display a small window without any VMs that can be executed since none are imported initially to run through this Virtual Box.</LI>
<LI>On successful completion of all the above steps, move to the next software installation below.</LI>
</OL>

## Cloudera Virtual Machine (VM)
<OL>
<LI>The latest version of Cloudera Virtual Machine (VM) - Quickstarts for local PCs can be downloaded from the following website: http://cloudera.com/downloads.html.

![image](https://cloud.githubusercontent.com/assets/19809692/25776640/f912c0fe-3291-11e7-8dc9-e542bf4b5f43.png)
</LI>
<LI>Click on <b>Quickstarts</b> to download one of the Quickstarts VM (current version available is 5.10)</LI>
<LI>Download the Quickstart VM zip file to the local PC and uncompress it to a directory from where it can be referenced easily.</LI>
<LI>Start/Open the Oracle Virtual Box installed previously. Using the <b>Import Appliance</b> option from its <i>File</i> menu, import the Cloudera Quickstart VM.</LI>
<LI>Once the VM is imported successfully, it will be displayed in the Virtual Box</LI>
<LI>Here is the view of the general setup of Cloudera VM from the Oracle Virtual Box:

![image](https://cloud.githubusercontent.com/assets/19809692/25776527/b0ed9838-328e-11e7-95f0-0fc5801d0543.png)
</LI>
<LI>Review all the setup options especially <i>memory</i> and <i>video memory</i> to ensure that the VM will run successfully on your local machine. For Cloudera VMs 5.5 and above, 8 MB of RAM should be allocated for decreased latency.</LI>
<LI>Click on <i>Start</i> button to run the VM within the Virtual Box. This step will take few minutes to complete depending on the available memory on the local PC and the amount of memory allocated to the VM.</LI>
<LI>This installation can be marked as complete once a new window opens up for the VM.</LI>
<LI>Terminal Window or Browser can be choosen as a couple of quick tools on the VM</LI>
<LI>Different Configuration options of the VM can be viewed on the browser by using the URL http://quickstart.cloudera:50070</LI>
<LI>To browser the Hadoop file system, choose the <i>Utilities</i> tab on the browser page, and pick the <i>browse the file system</i> option.</LI>
</OL>
