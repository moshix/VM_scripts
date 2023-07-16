# VM Scripts
Stuff for every day use when you manage a VM mainframe



      
<br><br><br>

# Some Useful VM Sorcery

How to enable PERFKIT on z/VM (including web interface): https://www.ibm.com/support/pages/system/files/inline-files/Configuring_Perfkit.pdf

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

