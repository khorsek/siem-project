# This is where I'll document my experience in setting up an Azure Sentinel (SIEM) and connect it to a live virtual machine acting as a honey pot. We will observe live attacks (RDP Brute Force) from all around the world. 

# We will use a custom PowerShell script to look up the attackers GeoLocation information and plot it on an Azure Sentinel Map.

# The purpose of this project is to become more familiar with cybersecurity tools and just develop my skills.

# I will be adding screenshots like the example below, to document this experience better.
<img src="images\test.PNG">

# Some of the tools that I will use in this lab will be: Microsoft Azure, Microsoft Sentinel, A VM set up in Azure, powershell to extract IP's and other information of attackers including geographic data, and Log Analytics Workspace.

# Starting off the lab, I first begin by creating a Microsoft Azure account(first 12 months is apparently free)
<img src="images\1.PNG">

# The next thing I'm gonna want to do is to create a virtual machine, which will serve as our "honeypot" and attract the attention from people all over the globe. I just name it something like "honeypot-vm" and proceed to deploy it:
<img src="images\2.PNG">

# With the VM made, it's time to create a Log Analytics workspace. Once I'm done with the configurations, I just go ahead and deploy it.
<img src="images\3.PNG">

# I navigate over to Microsoft Defender for cloud and enable the defender for servers and for it to store security events.
<img src="images\4.PNG">

# After enabling that, I connect my VM to log analytics workspace.
<img src="images\5.PNG">

# Now to set up Azure Sentinel. I just go to the dashboard and add it to my workspace:
<img src="images\6.PNG">

# I then log into my VM via Remote Desktop Connection and get a first glimpse of my VM.
<img src="images\7.PNG">

# To simulate an intruder trying to log in and get a look at what it looks like, I purposefully fail a login attempt and check it in event viewer.
<img src="images\8.PNG">

# I'm gonna disable the firewall on the VM so things can start getting interesting. I disable it in firewall settings and give it a little ping from my host machine to test it out.
<img src="images\10.PNG">

# Now, I'm gonna download a powershell script that will take the IP captured in Event Viewer, put it through a geolocation api and relay the results back to me so. I copy the script and save it into Powershell ISE(Github link below)
<img src="images\11.PNG">

<a href="https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1">Powershell script</a>

# I then open this <a href="https://app.ipgeolocation.io/">link</a> and sign up to get an api key.
<img src="images\12.PNG">

# Once I plug in the api key, I launch the script and already notice two attempts(ones I did earlier) as the script basically goes through the event viewer for a cert ID(failed logins) and run them to the api, where it spits out the data on screen and in a separate folder. In the log, the first few logs are a sample but the last 2 ones are real.
<img src="images\14.PNG">
