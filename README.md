# secured-transfer
## Introduction
Secured-Transfer is a FTP based client tool which automatically encrypts the file content by random generated AES key.
The AES key is encrypted by the receiver's RSA public key.

## Requirement
- windows 10 OS
- Make sure local port 8080 is available (This port is for local use only)
- If you want to have a backup copy running in a different machine, make sure the backup machine has the same Domain under HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters in windows Registry.

## User Manual
#### Quick Start
    
###### For Client User of IBC
1. Place Secured-Transfer-win32.zip into a disk which has enough space and unzip it.
2. Launch the app via Secured-Transfer.exe and login. Keep it running(don't quit it). Wait 2 minutes for first time initialization.
3. Drag and drop the files you want to send to ibc into /2019xxxx/ folder.
4. App also automatically downloads files into \Secured-Transfer-win32-x64\resources\app\data\Download\
5. **Backup** \Secured-Transfer-win32-x64\resources\app\data\db.sql
6. auto upload: Put upload file under \Secured-Transfer-win32-x64\resources\app\data\Upload\2019xxxx\
Note: upload file will be placed in the ftp server folder \2019xxxx\. Avoid duplicate filename by adding timestamp.

###### Check File Transfer Log File
Use Notepad++ to open \Secured-Transfer-win32-x64\resources\app\data\log.txt
and set it auto-reload by configuration.

Settings -> Preferences -> MISC. -> Update silently
![image](https://i.stack.imgur.com/WTQo1.png)

#### Advanced Feature
1. Backup Copy: If you want to have a backup copy running in a different machine. 
   - After the first time launch, everything works properly, copy the whole folder \Secured-Transfer-win32-x64 into your backup computer. 
   - Make sure the backup machine has the same Domain under HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters in windows Registry. 

2. Secured-Transfer provides key management function which can replace the pair of RSA key. In "Self-Key" row,
click "regenerate" icon will re-generate a new pair of key. And it will automatically send to the other involved parties.
It needs approximate 10 minutes to take it into effect.


#### Note
###### Browser Cache Location
C:\Users\<UserName>\AppData\Roaming\Secured-Transfer
If you want to clean everything. Delete this browser cache and installation folder Secured-Transfer-win32-x64/.

## Security Spec
- Transport Layer: FTP over TLS support 
- AES key size: 256 bit
- RSA key size: 2048 bit (2048-bit keys are sufficient until 2030)
- RSA private key are encrypted by specific machineID of the device and stored in local DB.
- Create separate accounts for each FTP user which provides additional security isolation.



