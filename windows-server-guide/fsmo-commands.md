# FSMO commands



{% code lineNumbers="true" fullWidth="true" %}
```powershell
Admin user has to be (Domain Admin, Scema Admin, administrator. Enterprice, domain users, group policy crententials )
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
from 2012 to 2019, mount dvd, 
CD Support, cd adprep, adprep.exe /forestprep
type c and enter 
varify. 
then 
adprep.exe /domainprep
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
To test the DNS server

dcdiag /test:dns /dnsall /v
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
dcdiag /e /test:sysvolcheck /test:advertising         # Test that sysvol is shared and advertising
dfsrmig /getglobalstate                               # Verify the system uses the FRS or DFRS

dfsrmig /setglobalstate 1                             # Waiting between 15 min to 1 hour

dfsrmig /getmigrationstate                            # Verify that all domain controllers have migrated successfully to the worldwide state

dfsrmig /setglobalstate 2                             # Waiting between 15 min to 1 hour

dfsrmig /getmigrationstate                            # Verify that all domain controllers have migrated successfully to the worldwide state 

dfsrmig /setglobalstate 3                             # Waiting between 15 min to 1 hour
dfsrmig /getmigrationstate                            # Verify that all domain controllers have migrated successfully to the worldwide state

net share                                             # Verify the SYSVOL share and type net share

Start - services.msc - check 'File Replication Service' Disabled and Stopped  --- OK
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Move-ADDirectoryServerOperationMasterRole -Identity "2022b" PDCEmulator

Move-ADDirectoryServerOperationMasterRole -Identity "dc1" PDCEmulator -Force
## 

Move-ADDirectoryServerOperationMasterRole -Identity "dc1" RIDMaster
Move-ADDirectoryServerOperationMasterRole -Identity "dc1" Infrastructuremaster
Move-ADDirectoryServerOperationMasterRole -Identity "dc1" DomainNamingmaster
Move-ADDirectoryServerOperationMasterRole -Identity "dc1" SchemaMaster

regsvr32 schmmgmt.dll

xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
all in one command to move fsmo (working)
Move-ADDirectoryServerOperationMasterRole -Identity destinationserver -OperationMasterRole SchemaMaster -Force
Move-ADDirectoryServerOperationMasterRole -Identity destinationserver 0,1,2,3,4

xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
 Move-ADDirectoryServerOperationMasterRole "DC02-2019" -OperationMasterRole PDCEmulator,RIDMaster,InfrastructureMaster,SchemaMaster,DomainNamingMaster -Confirm:$false
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
dsa.msc = Active Directory Users & Computers 

dfsrmig /getmigrationstate
dfsrmig /getglobalstate

#in old serves (2012 or 2018) dfsrmig.exe /setglobalstate 0 , 1, 2, 3 
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

Domain & Trust, right operation master , change 
active directory users and computers, righgit click on domain , operation master (rid pdc, infrastructure)
for schemamaster (regsvr32 schmmgmt.dll ) then mmc
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
backup of sysvol 
gpo 
sysvol folders
ADSI = edit sysvol subscription and give msdfsr disable, and value 0 
then powershell 
repadmin /syscall masterserver /APed
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Invoke-Command -ComputerName PDCServername, ADCserverName, ADCserverName2 -ScriptBlock {Stop-Service DFSR}
Invoke-Command -ComputerName PDCServername, ADCserverName, ADCserverName2 -ScriptBlock {Start-Service DFSR}

repadmin /syncall pdcname /APed

eventviewer 4602 event number means dfsr server started working
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
{% endcode %}
