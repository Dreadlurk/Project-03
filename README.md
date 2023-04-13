Esteban Cardoza
CEG2410


Install AD through powershell used as a guide: (https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-)
In server manager: add roles and features and followed prompts to create DNS
labeled tree as ad.xeno.com
selected the server ip 10.0.0.134
selected features to be added (default)
installed.



Created OU in active directory ----

![OU computers](https://user-images.githubusercontent.com/122290600/231658438-cf3d0dad-67cc-44ac-8672-24ed0f22b4ee.PNG)


![OU servers](https://user-images.githubusercontent.com/122290600/231658451-4d03319c-29b8-45cf-9414-3e827baacc89.PNG)

![OU users](https://user-images.githubusercontent.com/122290600/231658459-5a7fe2bd-da48-4e4a-b2f3-058143c218e1.PNG)






Made a seperate aws instance of windows and changed the dns to match the first one.
pinged to verify connection.

Changed IP address to manual and entered in elastic IP address that was defined on AWS

Had to change security policy on AWS to allows all traffic from 10.0.0.0/16 (cheating?)

pinged to verify connection.


![connectivity](https://user-images.githubusercontent.com/122290600/231658286-101f4ecc-0227-4770-a61d-d960d1f04fc2.PNG)





Added users to respective OU's-----

![add HR](https://user-images.githubusercontent.com/122290600/231658547-62edacbd-98a1-4c35-bcd8-9df5c00f9076.PNG)
![add develop](https://user-images.githubusercontent.com/122290600/231658548-0c14e124-d1ee-42a8-903e-0648f24ddeea.PNG)
![add engineers](https://user-images.githubusercontent.com/122290600/231658549-9f9a02e4-8559-4a53-b8a5-c42a3f55d7fa.PNG)
![add finance](https://user-images.githubusercontent.com/122290600/231658551-66eca740-6588-4637-b3ff-9527ec4923a1.PNG)




Created groups and placed them in the respective OU's based on rules

project_repos_RW - servers
finance_RW - finance 
onboarding_R - HR
server_access - xeno Server
dev_eng_admins - Users
hr_finance_admins - Users
remote_workstation - workstatiosn




GPO ----

workstation 15 min lockout - GPO/computer config/policies/windows settings/ security settings/local polices/ security options
define 15 min in seconds
WORKSTATIONS


Prevent execution of programs on computers in Secure OU 
GPO/user config/ admin templates/ system/ Don't allow specified windows app
define what applications to prevent for group]
ALL BUT HR,FINANCE, SECURE

Disable Guest account login to computers in Secure OU
GPO/computer config/polices/windows settings/security settings/ security options/ accounts guest account status
define as disabled and apply to group
SECURE

Allow server_access to sign on to Servers
GPO/computer config/ polices/ windows settings/ local polices/user rights assignment/ allow log on on as a service define
SERVERS

Set Desktop background for Conference computers to company logo 
GPO/computer config/ admin tmeplates/ personalization/ force specfice background and accent color
define logo picture
CONFERENCE

Allow users in remote_workstation group to RDP to Workstations 
GPO/computer config/windows settings. security settings/local polices/ windows components/ remote desktop services/ connections/ allow users to connect remotly by using RDS
enable
USERS


Managing OU ----

Added Bob Baker to hr_finance_admins and added a new user Johnny Boak to eng_dev_admins.


Delegated control of Finance and HR OU to Bob Baker by in active directory users and computers: delegate control wizard and added Bob Baker to
allow  for all common tasks such as: create delete and manager user accountts, read all user information, create delete and manage groups, modify the membership of groups.
I believe reset user poasswords and change on  next logon should be reserved for IT admins.


Delegated control of Developers to the new employee Johnny Boak by delegating OU in active directory users and computers: delegate control wizard and added Johnny Boak to
allow for all common tasks such as: create delete and manager user accountts, read all user information, create delete and manage groups, modify the membership of groups.
I believe reset user poasswords and change on  next logon should be reserved for IT admins.



