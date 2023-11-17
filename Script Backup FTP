add dont-require-permissions=yes name=Backup-ftp owner=abbio90 policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon source="{\
    \n#----------------------------------------\
    \n# SAVE BACKUP TO FTP SERVER BY foisfabio.it\
    \n# \
    \n# Script:  Save Backup FTP SERVER\
    \n# Version: 1.0\
    \n# RouterOS v.7.9.2\
    \n# Created: 14/11/2023\
    \n# Updated: --/--/----\
    \n# Author:  Fois Fabio\
    \n# Editor: Fois Fabio\r\
    \n# Website: https://foisfabio.it\
    \n# Email: consulenza@foisfabio.it\
    \n#\
    \n#----------------------------------------\
    \n\r\
    \n#COMPILARE LE VARIABILI SOTTOSTANTI:\
    \n\
    \n#INSERIRE INDIRIZZO SERVER\"\
    \n:local ipserver \"10.250.159.100\"\
    \n\r\
    \n#----------------------------------------\r\
    \n\r\
    \n#INSERIRE CREDENZIALI D'ACCESSO\r\
    \n:local user \"admin\"\
    \n:local password \"password\"\
    \n\r\
    \n#----------------------------------------\
    \n\r\
    \n#INSERISCI PERCORSO FILE DI DESTINAZIONE\
    \n:local dstpath \"/BACKUP_PROXMOX/Mikrotik/\"\r\
    \n              \
    \n#----------------------------------------\
    \n\
    \n \
    \n\
    \n############## Don\92t edit below this line ##############\r\
    \n\r\
    \n:local scheduleName \"Backup-FTP\"\r\
    \n:local myRunTime \"10d 00:00:00\"\r\
    \n\r\
    \n:if ([:len [/system scheduler find name=\"\$scheduleName\"]] = 0) do={\r\
    \n\r\
    \n    /log error \"[Backup-FTP] Alert : lo Scheduler non esiste. Creo lo scheduler\"\r\
    \n\r\
    \n\r\
    \n\r\
    \n    /system scheduler add name=\$scheduleName interval=\$myRunTime start-date=Jan/01/1970 start-time=startup on-event=\"/system script run Backup-ftp\"\r\
    \n\r\
    \n\r\
    \n\r\
    \n    /log warning \"[Backup-FTP] Alert : Scheduler creato .\"\r\
    \n\r\
    \n}\
    \n\
    \n:local sysname [/system identity get name]\
    \n\
    \n:local textfilename\
    \n\
    \n:local backupfilename\
    \n\
    \n:local time [/system clock get time]\
    \n\
    \n:local date [/system clock get date]\
    \n\
    \n:local newdate \"\";\
    \n\
    \n:for i from=0 to=([:len \$date]-1) do={ :local tmp [:pick \$date \$i];\
    \n\
    \n:if (\$tmp !=\"/\") do={ :set newdate \"\$newdate\$tmp\" }\
    \n\
    \n:if (\$tmp =\"/\") do={}\
    \n\
    \n}\
    \n\
    \n#check for spaces in system identity to replace with underscores\
    \n\
    \n:if ([:find \$sysname \" \"] !=0) do={\
    \n\
    \n:local name \$sysname;\
    \n\
    \n:local newname \"\";\
    \n\
    \n:for i from=0 to=([:len \$name]-1) do={ :local tmp [:pick \$name \$i];\
    \n\
    \n:if (\$tmp !=\" \") do={ :set newname \"\$newname\$tmp\" }\
    \n\
    \n:if (\$tmp =\" \") do={ :set newname \"\$newname_\" }\
    \n\
    \n}\
    \n\
    \n:set sysname \$newname;\
    \n\
    \n}\
    \n\
    \n:set textfilename (\$\"newdate\" . \"-\" . \$\"sysname\" . \".rsc\")\
    \n\
    \n:set backupfilename (\$\"newdate\" . \"-\" . \$\"sysname\" . \".backup\")\
    \n\
    \n:execute [/export file=\$\"textfilename\"]\
    \n\
    \n:execute [/system backup save name=\$\"backupfilename\"]\
    \n\
    \n#Allow time for export to complete\
    \n\
    \n:delay 2s\
    \n\
    \n:local time [/system clock get time]\
    \n\
    \n/tool fetch address=\$ipserver src-path=\$textfilename  user=\$user password=\$password port=21 upload=yes mode=ftp dst-path=(\$dstpath.\"/\".\$textfilename)\r\
    \n\r\
    \n:delay 10s;\r\
    \n\r\
    \n/tool fetch address=\$ipserver src-path=\$backupfilename  user=\$user password=\$password port=21 upload=yes mode=ftp dst-path=(\$dstpath.\"/\".\$backupfilename)\
    \n\
    \n\
    \n\
    \n\
    \n#Allow time to send\
    \n\
    \n:delay 5s\
    \n\
    \n \
    \n\
    \n#delete copies\
    \n\
    \n/file remove \$textfilename\
    \n\
    \n\
    \n/file remove \$backupfilename\
    \n\
    \n }\
    \n\
    \n\
    \n\
    \n"
