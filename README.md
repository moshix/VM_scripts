# VM Scripts
Stuff for every day use when you manage a VM mainframe



      
<br><br><br>

# Some Useful VM Sorcery

How to enable PERFKIT on z/VM (including web interface): https://www.ibm.com/support/pages/system/files/inline-files/Configuring_Perfkit.pdf  
This guide works from z/VM 6.1 up to z/VM 7.3 and the manual is in this repo.  

To rebuild failed saved segments CMSPIPES CMSBAM etc etc, do this as MAINT640:
<pre>
q filepool connection for maint640 vmpsfs:
put2prod savecms
put2prod segments all
vmfbld list segbld esasegs zvmsegs blddata ( All
</pre>

If the system name of z/VM has changed follow this:

<pre>
https://www.ibm.com/docs/en/zvm/7.2?topic=system-changing-name-target

Changing the Name of the Target System
Last Updated: 2023-09-22
To perform the steps in this procedure, the target system should be IPLed second level on the running system.

On the target system, log on to the MAINTvrm user ID.
Select a name for the target system. The target system name must be 1-8 alphanumeric characters and must start with a letter. The name must not contain any blanks and must not be NOSYS or NOSSI.
Update the System-Level Product Inventory Table (VM SYSPINV file) with the target system name.
Link to and access the PMAINT 41D minidisk in write mode:
link to pmaint 41d as 41d w
access 41d fm

Make a backup copy of the VM SYSPINV file. XEDIT the VM SYSPINV file using the WIDTH option to avoid accidentally truncating data in the file:
xedit vm syspinv (width 120

Change all occurrences of the source system name (on the :SYSTEM. tag) to the target system name. Save the updated VM SYSPINV file.
Do not release the PMAINT 41D minidisk. There are additional files on the PMAINT 41D minidisk that must be updated.
Update the SYSTEM CONFIG file with the target system name.
Link to and access the PMAINT CF0 minidisk in write mode:
link to pmaint cf0 as cf0 w
access cf0 fm

Make a backup copy of the SYSTEM CONFIG file. XEDIT the SYSTEM CONFIG file and change all occurrences of the source system name to the target system name. Save the updated SYSTEM CONFIG file.
Run the CPSYNTAX utility to validate the changes you made to the SYSTEM CONFIG file:
cpsyntax system config fm

Update the VMSES/E service inventory files with the target system name.
Access the MAINTvrm 51D minidisk.
Make a backup copy of the System-Level Service Update Table (VM SYSSUF file). XEDIT the VM SYSSUF file using the WIDTH option to avoid accidentally truncating data in the file:
xedit vm syssuf (width 120

Change all occurrences of the source system name (on the :PRODLEV. tag) to the target system name. Save the updated VM SYSSUF file.
Locate and make backup copies of all the Service-Level Production Status Tables (files with a file type of SRVPROD) on the MAINTvrm 51D and PMAINT 41D minidisks. XEDIT each SRVPROD file using the WIDTH option to avoid truncating data in the file:
xedit compname srvprod (width 120

Change all occurrences of the source system name (on the :STAT. tag) to the target system name. Save the updated SRVPROD file.
Log off the MAINTvrm user ID.
Update the recovery SFS server with the target system name.
Log on to the recovery SFS server, VMSERVR. The default password is WD5JU8QP.
To stop the server, enter:
STOP

XEDIT the VMSERVR DMSPARMS file (located on the VMSERVR 191 minidisk). Change the LUNAME to match the target system name. Save the updated file.
Update the CRR log minidisk with the target system name:
fileserv crrlog 306 307

Note: 306 and 307 are the default minidisks for the recovery server defined in the VMSYSR POOLDEF file (the VDEVS assigned on the DDNAME=CRRn statements) on the VMSERVR 191 minidisk. If your recovery server is using minidisks other than the defaults, adjust the FILESERV command accordingly.
Restart the server and disconnect:
profile
#cp disc

Log on to the MAINTvrm user ID.
Shut down the target system:
shutdown

When the shutdown is complete, IPL CMS on your first-level system user ID.
While logged on to your first-level system user ID, rewrite or remove the system ownership information on your spool and page volumes.
For each spool and page volume, enter the following command:

cpfmtxa vol_addr vol_label owner nossi target_system_name

You will be prompted to confirm the format command. When cylinder 0 is formatted, you need to reenter the allocation for the volume.
For a spool volume, enter:
spol 0-end
end

For a page volume, enter:
page 0-end
end

IPL the target system second-level to make sure it will come up. Enter the following sequence of commands:
system clear
term conmode 3270
q cons

Note the console address. If the address is not 20, redefine it:
define conaddr 20

IPL the system:
ipl target_RES_addr clear loadparm 20

Check the SAPL screen to make sure the IPL parameters are correct. Add cons=20 to the IPL parameters and press F10 to complete the load. You will be running on the OPERATOR user ID.
You are now ready to start using the target system. You can shut it down if you intend to bring it up in another location, such as its own LPAR or another user ID. Otherwise, disconnect from the OPERATOR user ID and log on to the MAINTvrm user ID.      
</pre>


Then, do this:
<pre>
link to maint 190 as 190 mr
access 190 K
put2prod savecms
put2prod segments all
service restart
service gcs bldnuc
put2prod
</pre>


New Delhi,  July 16, 2023    

