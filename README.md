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
- Elasticsearch
- Cassandra
- Kali Linux
- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.



Lets Begin with installing Sysmon to our Windows 11 VM enviorment.
-------


+ Before beginning we need to meet some requirments before we can start .First thing is we need to have a VM environment to host the Windows 11 machine and the  other applicaion we are going to be using.(Im  using Promox VE) 

+ So if you have your VM envirnment the next step is having Windows 11 ISO installed.if you dont install it and then we can begin.

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


Now let's install Wazuh and TheHive 
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
 
  + The second way to run the server is by using powershell and using the the IP address and ssh command:
     
    1.Make sure you copy the public IP address of the server and in powershell lets use the ssh command so ssh root@the IP address for the Wazuh server.( Mine  was )
    
    2.Once SSHing into your Wazuh server you will need to login with your password which you will find it on the MyDFIR-Wazuh page copy the password and right click an paste it in the powershell termial.Onced logged in lets clear the screen and update and upgrade all of our     repositories by typing apt-get update  && apt-get upgrade -y and hit enter.
    
    
  - Now will that downloading let install Wazuh so lets go to your internet browser and search up Wazuh and click on install Wazuh.On the install page scrool down and click Quickstart button and it will take you to the quickstart page.
 
  - Scroll down and you will see Install Wazuh copy the command there on the page and paste in the command in the powershell.
 


- Now lets do the same thing for our Hive server do lets open up our hive server copy our server IP address and lets open up a new powershell and  type shh root@IPaddress and type yes and the password for that server.

- Now lets update and upgrade all of the respositories by typing apt-get update  && apt-get upgrade -y and hit enter.

(Once both are done installing make sure you take note of the username ande passwords for both server)

- now lets try connecting to our server using our web browser.

- lets type https//: and the public IP address of the server.

- Here you may get a error of site cant be reached so lets troubleshoot it 

+ So first thing is to check to see if there is any firewall that is preventing us from accessing the site.

  1. So lets go to our server and click on settings and find firewall   on the left siode of the page. check to see if there is any if not go to the next step.
 
  2. Now lets check the host firewall. but before we need to check to see that the server is up and running. So in Powershell you want to type systemctl status wazuh- the hit tab twice. Now you will see the service that you have running but we want to look at the manger          service so hit the up arrow key and type manger and the end of the command and enter.(you should see that is active and running but still arent able to connect to the server if so continue to the next step)
     
  3.  SO now we need to make modifications to our host firewall so let go to our powershell clear the screen by type clear and now we ant to allow an traffic thats coming in on port 443 so type ufw allow 443.
 
  - Now let check the internet browser to see if the server is accessable. (You should be able to access the server just click advance in the error page)

  - Now Wazuh server should be able lets log in using our creditional from earlier.


  - Now lets look at our Hive server and let install it lets head over to StrangeBee .com

  - So there you will see Step by Step instruction  to install TheHive. (Make sure its DEB configureation)

  - So copy and and paste the commands in the powershell starting with Step 1.

    + Step 1: Install required dependencies
   
    + Step 2: Set up the Java virtual machine (JVM)
      - Run the following commands:

wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto.gpg
echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" | sudo tee -a /etc/apt/sources.list.d/corretto.sources.list
** sudo apt update
** sudo apt install java-common java-11-amazon-corretto-jdk
echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment
export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"

- Verify the installation.

java -version

+ You should see output similar to the following:

openjdk version "11.0.28" 2025-07-15
OpenJDK Runtime Environment Corretto-11.0.28.6.1 (build 11.0.28+6-LTS)
OpenJDK 64-Bit Server VM Corretto-11.0.28.6.1 (build 11.0.28+6-LTS, mixed mode)

If a different Java version appears, set Java 11 as the default using sudo update-alternatives --config java.


  + Step 3: Install and configure Apache Cassandra
    - Add Cassandra repository references.

a. Download Cassandra repository keys.

wget -qO -  https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor  -o /usr/share/keyrings/cassandra-archive.gpg

b. Check if the /etc/apt/sources.list.d/cassandra.sources.list file exists. If it doesn't, create it.

c. Add the repository to your system by appending the following line to the /etc/apt/sources.list.d/cassandra.sources.list file.

echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 41x main" |  sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list


- Update your package index and install Cassandra using the following commands:

sudo apt update
sudo apt install cassandra

- Verify that Cassandra is installed.

sudo systemctl status cassandra

This will show you the status of the Cassandra service. If Cassandra is installed you should see details about the service. If it's not installed, you will receive an error indicating that the service isn't found.

By default, data is stored in /var/lib/cassandra. Set appropriate permissions for this directory to avoid any issues with data storage and access.

sudo chown -R cassandra:cassandra /var/lib/cassandra


-Next step in Cassandra is to configure and run it but we will come back to that later lets finish install the other necessary software.


  + Step 4: Install and configure Elasticsearch

-Add Elasticsearch repository references.

a. Download Elasticsearch repository keys.

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch |  sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
sudo apt-get install apt-transport-https

b. Check if the /etc/apt/sources.list.d/elastic-8.x.list file exists. If it doesn't, create it.

c. Add the repository to your system by appending the following line to the /etc/apt/sources.list.d/elastic-8.x.list file.

echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" |  sudo tee /etc/apt/sources.list.d/elastic-8.x.list 

- Update your package index and install Elasticsearch using the following commands:

sudo apt update

sudo apt install elasticsearch


+ Step 5: Install and configure TheHive

- Download the installation package along with its SHA256 checksum and signature files.

Using wget:

wget -O /tmp/thehive_5.6.2-1_all.deb https://thehive.download.strangebee.com/5.6/deb/thehive_5.6.2-1_all.deb
wget -O /tmp/thehive_5.6.2-1_all.deb.sha256 https://thehive.download.strangebee.com/5.6/sha256/thehive_5.6.2-1_all.deb.sha256
wget -O /tmp/thehive_5.6.2-1_all.deb.asc https://thehive.download.strangebee.com/5.6/asc/thehive_5.6.2-1_all

  - Install the package.

   Using apt-get to manage dependencies automatically:

  sudo apt-get install /tmp/thehive_5.6.2-1_all.deb.




Configuring TheHive and Wazuh
-----------
- In the hive powershell we need to configure cassandra so lets type nano/etc/cassandra/cassandra.yaml.

- Once inside of the file first we want to change ther cluster name to whatever you would like.(I changed mine to mydfir)

- Now we need to change our listen address so to find a word that you want to look up just hold down the control W and type listen  and it should lead you to the listen_address where you will its listed as localhost.Lets replace it with TheHive publuic IP Address which you can get from Vultr.

- Lets look up the RPC_address using control W and we are replacing it with the public Ip address of the Hive once again.

- Lastly lets look up the seed using control w and replace that with the Hive public IP address aswell.

- Save and exit the file by holding control X and hit Y to save and exit. Next stop the service of Cassandra by typing systemct1 stop cassandra and tab for auto completion and enter.

- Next thing is to remove any of the remaining files under cassadra libary by typing rm -rf /var/lib/cassandra/*. After that lets start and look up the status of our service.

- Now lets configure our elastic search so lets type nano /etc/elasticsearch/elasticsearch.yml.Find Cluster name  and change the name to whatever you like.(I changed mine to mydfir)

- next lets down to node name asnd remove the  # to remove as a comment then scroll down to network host  and remove the # there aswell  and past the Public IP address for the hive.

- Find http.port remove that # and then find cluster.intial and remove that #  but here we want to remove node 2 and leave node 1.

- Save and exit that file.Run the command systemctl start elasticsearch enter and then systemctl enable elasticsearch and then the status by typing systemctl status elasticsearch





