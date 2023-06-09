# This is where I'll document my experience in setting up a Microsoft Sentinel (SIEM) and connect it to a live virtual machine acting as a honey pot. We will observe live attacks (RDP Brute Force) from all around the world. 

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

# As a quick test, I try to login again via RDP and can instantly see the new entry as seen below:
<img src="images\15.PNG">

# Now what I'm going to do is go back to Log Analytics Workspace, and create a custom log to tell analytics what to look for. I copy and paste the contents of the log into a text file on my host machine and upload it as seen below:
<img src="images\16.PNG">
<img src="images\17.PNG">

# While I wait to see the custom log get deployed, I test out the Logs tab in log analytics to run a SecurityEvent Log and I can already see so many different IP's scanning my VM as seen below:
<img src="images\18.PNG">

# Once my custom log gets initialized, I test it out by running a query on it and it works! I got a full list of all the IP's up till that point.
<img src="images\19.PNG">

# Now it's time to extract fields from the raw custom log data and give them each its own field name, with latitude as an example:
<img src="images\20.PNG">

# The point of this is to train the algorithm to recognize and categorize each field appropriately. Another example given for the "username" field.
<img src="images\21.PNG">

# Now that the fields have been categorized, I rerun the query and now every subsequent request will be categorized according to each fields as seen below.
<img src="images\22.PNG">

# Just realized that I forgot to add some of the other fields in. Should be good now.
<img src="images\23.PNG">

# With that out of the way, it's time to set up the map in Sentinel. I open up a new workbook and type in the following code to not select any empty requests or ones that are "samplehosts" and I get the following to start with:
<img src="images\24.PNG">

# With that working, I switch visualization to map, set the size to full, and set the location info using latitude/longitude and I get the following result!
<img src="images\25.PNG">

# Now with everything set, all I have to do now is just wait and let people hammer at my VM. Then I'll be able to see all bad guys.

# I will probably keep this running until I get multiple countries attacking and I'll keep updating this all throughout with pictures.

# I wish I could keep it up for a really long time but I don't want to risk a charge to my Azure account.

# As of 3:05 PM on 5/1/2023, the map looks like the following:
<img src="images\26.PNG">

# As of 3:51 PM, the map looks like the following(finally got one from Russia):
<img src="images\27.PNG">

# Last update. As of 8:17 PM, this is what the map looks like now:
<img src="images\28.PNG">