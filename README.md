# SOC Automation Lab

## Objective
The Detection Lab project aimed to establish a controlled environment for simulating and detecting cyber attacks. The primary focus was to ingest and analyze logs within a Security Information and Event Management (SIEM) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies.

### Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- Windows 11 IOS
- Sysmon
- Wazuh
- Kali Linux
- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.



Lets Begin with installing Sysmon to our Windows 11 VM enviorment.
-------


+ Before beginning we need to meet some requirments before we can start .First thing is we need to have a VM environment to host the Windows 11 machine and the  other applicaion we are going to be using.(Im  using Promox VE) 

+ So if you have your VM envirnment the next step is having Windows 11 ISO installed.if you dont install it and then we can begin/

+ In your Windows 11 Vm lets start by downloading Sysmon so lets open up the internet browser and search Sysmon.Click the top link should be a Microsoft link.Once on the next page scroll down until you see the "Download Symon" link click it and the download should begin creating a Sysmon folder. 

+ After downloading the Sysmon folder now we need to gather the sysnom configuration so to do that go to back to your internet browser and search for sysmon config Olaf.CLick the link (Should be a github link).

+ Scroll all the way down to where you will find sysmon config.xml and click it.Inside the you will find Raw (which is the raw data that you need) so after clicking Raw right click anywhere onthe page and save as and save it into your Sysmon folder.

+ Now that you have Sysmon and its Configuration let open search powershell in the search bar on the start tab and right click powershell and find run as Administrator. On the pop up click yes to contiue. 

+ in the Sysmon folder lets click on the search bar and it should highlight the whole file path you want to copy it and  head to powershell.

+ In powershell lets type cd (which mean Change Directory) and paste the folder path that you just copied.YOu should be in the same directory of your Sysmon and you can check by typing pwd and ls to check the files. 

+ Alright lets install Sysmon now so to do that just type in Symon and hit the tab key to use the auto completion feature.(Make sure you have pointing sysmon .exe or sysmon64.exe either one is fine im going to use sysmin64.exe).So at the end of that line you want to type -i (which mean install) and then you want to point it to sysmon config file.(So it should look like the picture below)

+ Hit enter and the install should begin hit Agree on the popup and it will install.

+ Make sure you check to make sure that sysmon is installed by either checking your system servics and look for sysmon64 and the ohter way is event viewer and clicking on applications and service logs then microsoft directory and the windows folder and scroll down until you see the sysmon folder open it an click operation and if its working you should see some logs.

+ Now create a snapshot of your current state just incase something happens.


Now let's install Wazuh
------

- So fisrt we need to create our servers for Wazuh so using going to be using vultr.(If you dont have account you can make on or their are plenty of other alternatives servers).

- So lets login if you gont have login create a account . Once inside lets click on Deploy and deploy new server.
  
- For the type select shared CPU and location select the loaction for you.

- Scroll down to select the specs and select 4vCPUs  and 8GB of memory.

- Click on configure software for the OS select Ubuntu 24.04 .

- Now for Server Name You can name it whatever you will like.( I'm naming mine MyDFIR-Wazuh ). and you dont need automatic backups and check the box. and deploy.

- After that we need to repeat the steps for another server.(for the Hive)the only diffence is that we whould need to change the specs to handle all Elastic Search and Cassandra for the database we will to choose a higher vCPU and memory.So select 6 vCPU and 16 GB.

- Now for OS select the same as above steps and name it (I'm naming mine MyDFIR -TheHive)Disable automatic backups aswell and deploy.

- you should have two severs now.So now we need to open and run our Wazuh server and lets run the server.It two ways you can run the server:

  + The first way to run the server is on the main screen of then Wazuh server page you can go and find the View Console show below.
 
  + the second way to run the server is by using powershell and using the the IP address and ssh command:
     
    1.make sure you copy the IP address of the server and in powershell  
  
  +
  
