# secured-transfer
## Introduction
Secured-Transfer is a FTP based client tool which automatically encrypts the file content by random generated AES key.
The AES key is encrypted by the receiver's RSA public key.

## Requirement
- windows 10 OS
- Make sure local port 8080 is available (This port is for local use only)
- Make sure the backup machine has the same MachineGuid under HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Cryptography

## Security Spec
- AES key size: 256 bit
- RSA key size: 2048 bit (2048-bit keys are sufficient until 2030)
- RSA private key are encrypted by specific machineID of the device and stored in local DB.
- Create separate accounts for each FTP user which provides additional security isolation.


## User Manual
#### Quick Start
###### For IBC User
1. Register an account in exavault.com and get the FTP server Info. 
   - For example: I registered a server named losphoenix.exavault.com with admin user losphoneix
2. Use admin user to login, create an operation user for App operation and its own folder. 
   - For example: I created a user named ibcuser and its root folder /ibc
3. Use admin user to login, create operation users and related folders for all the other companies under /ibc folder.
   Send the credentials to them via secured channel.
   - For example: I created a user named company1 and its root folder /ibc/company1. 
    
4. Place Secured-Transfer-win32.zip into a disk which has enough space and unzip it.
5. **Modify \Secured-Transfer-win32-x64\resources\app\data\config\config.json** <br>
    ```Change "ftp_server_root_folder":"/"  to "ftp_server_root_folder":"/*"```
6. Launch and app and login by ibc user, keep it running(don't quit it). Wait 2 minutes for first time initialization.
7. The app automatically generates timestamp folder everyday based on Chicago's time.
8. Drag and drop the files you want to send to companyX into /companyX/2019xxxx/ folder.
9. App also automatically downloads files into \Secured-Transfer-win32-x64\resources\app\data\Download\
10. Backup \Secured-Transfer-win32-x64\resources\app\data\db.sql
11. Auto upload: Put upload file under \Secured-Transfer-win32-x64\resources\app\data\Upload\{CompanyName}\2019xxxx\. 
Note: upload file will be placed in the ftp server folder \{CompanyName}\2019xxxx\. Avoid duplicate filename by adding timestamp. 
    
###### For Normal User
1. Place Secured-Transfer-win32.zip into a disk which has enough space and unzip it.
2. Launch and app and login by companyX user, keep it running(don't quit it). Wait 2 minutes for first time initialization.
3. Drag and drop the files you want to send to ibc into /2019xxxx/ folder.
4. App also automatically downloads files into \Secured-Transfer-win32-x64\resources\app\data\Download\
5. **Backup** \Secured-Transfer-win32-x64\resources\app\data\db.sql
6. auto upload: Put upload file under \Secured-Transfer-win32-x64\resources\app\data\Upload\2019xxxx\
Note: upload file will be placed in the ftp server folder \2019xxxx\. Avoid duplicate filename by adding timestamp.

###### Check File Transfer Log File
Use Notepad++ to open \Secured-Transfer-win32-x64\resources\app\data\log.txt
and set it auto-reload by configuration.

Settings -> Preferences -> MISC. -> Update silently
[https://i.stack.imgur.com/WTQo1.png]

#### Advanced Feature
1. Secured-Transfer provides key management function which can replace the pair of RSA key. In "Self-Key" row,
click "regenerate" icon will re-generate a new pair of key. And it will automatically send to the other involved parties.
It needs approximate 10 minutes to take it into effect.

#### Note
###### Browser Cache Location
C:\Users\<UserName>\AppData\Roaming\Secured-Transfer
If you want to clean everything. Delete this browser cache and installation folder Secured-Transfer-win32-x64/.



## Command For Debug & Testing 

##### Download electron-packager
npm install electron-packager -g


##### Launch testing Browser
- FOR MAC:
open -a Google\ Chrome --args --disable-web-security --user-data-dir
- FOR WINDOWS:
C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --disable-web-security --user-data-dir=c:\
##### Check Running Port:
sudo lsof -PiTCP -sTCP:LISTEN



## Feature

For IBC:
- After creating a new account for company1 and corresponding folder (such as /ibc/company1). The App will automatically
  generate a IBC_Key.txt under the company1 folder for the company1 user to get the public key of IBC.
- Everyday App will automatically create a new folder named /ibc/*/YYYYMMDD/ based on Chicago local time.
- To send files to company1, just upload the files into /Company1/YYYYMMDD/folder, the app will automatically encrypt 
  the files using the public key of Company1. 
- App will automatically download files into local.    


For Company1:

- After login using the app, it will automatically genreate a Pub_Key.txt under its root folder for IBC user to get the public key.
- To send files to IBC, just upload the files into /YYYYMMDD/ folder, the app will automatically encrypt 
  the files using the public key of IBC. 
- App will automatically download files into local.

For Re-generating Key Pair:

- Under key management, clicking "regen" icon will regenerate key-pair. 

