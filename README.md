# Agent-Based-Monitoring-Linux

I will demonstrate how to set up and run local agent-based scans on tenable.io for Windows devices.

<h2>Requirements for this Lab</h2>

- <b> Azure </b>
- <b> Ubuntu Server 22 VM </b> 
- <b> Tenable.IO </b>

<p align="center">
After creating a Linux virtual machine, I will now show you how to create a local-based agent to scan my Linux device. The purpose of this is that in work settings, employees are given devices to work on outside of the office. Instead of having to manually check all of those devices, I can set up an agent that does a self-assessment on the device it's installed on.

<h2> 1. While my VM is booting up, I will log onto Tenable. </h2>

<h3> Now I'm gonna create the Agent Group </h3>

Agent groups are used to organize and manage the agents linked to your account. Each agent can be added to any number of groups, and scans can be configured to use these groups as targets.

Go to:

* Settings
* Sensors
* Nessus Agents
* Agent Groups

<img src="https://i.imgur.com/HuAt33V.png" height="80%" width="80%" alt="Agent Group created"/>
<br />

> As you can see, the Agent Group above was created.

<h2> 2. Next, I'm going to create a Basic Agent Scan </h2>

* Go to Scanner
* Press New Scan
* Press the Nessus agent
* Then select the Basic Agent Scan Template

This is what it looks like when you get there.

<img src="https://i.imgur.com/AIqjFK6.png" height="80%" width="80%" alt="Agent Group created"/>
<br />

Configurations:
* I selected the group I created in the "agent groups" section.
* Then I named the scan
* For the Scan Type, I am selecting a triggered scan so that when a filename I select ends up in the directory, & the agent discovers it, it will know it's time to scan.

For this scenario, the key file will be 
> goub.txt

<h3> I will now leave the scan as is and save it. </h3>

<h2> Now it's time to log into the Virtual Machine from Terminal. </h2>

<img src="https://i.imgur.com/bn8vfwR.png" height="80%" width="80%" alt="Agent Group created"/>
<br />

To SSH into the Linux computer, the command is 

> ssh username@publicIP
>, then enter the password of the computer

<h3> As you can see, I have successfully SSH'd into the VM. </h3>

<img src="https://i.imgur.com/m7nyqrd.png" height="80%" width="80%" alt="Agent Group created"/>
<br />

<h2> I am now going back to Tenable to add the Nessus Agent.</h2>

<h3> Copy and paste this command from settings in Tenable. This is the code that will be used to install the agent on the VM. </h3>

* Settings
* Sensors
* Nessus Agents
* Linked Agents
* Look at the instructions for installing the Agent on Windows Platforms

It should appear on the right side of the screen.

<img src="https://i.imgur.com/XWC81C6.png" height="80%" width="80%" alt="Agent Group created"/>
<br /

Copy and paste the command as seen:

> curl -H 'X-Key: 58aab372289ac80911e4c5ad40a07b23b5524319f9ff5c010aa50ec625ccf389' 'https://sensor.cloud.tenable.com/install/agent?name=agent-name&groups=agent-group' | bash

I know have to edit the command to match my machines so it properly knows what to scan.

After editing, the new command should read

> curl -H 'X-Key: 58aab372289ac80911e4c5ad40a07b23b5524319f9ff5c010aa50ec625ccf389' 'https://sensor.cloud.tenable.com/install/agent?groups=Linux-Agent-Goub' | bash

* I removed the agent name, since we only have 1 VM in the group. As long as I include the agent group name, it will scan all devices in that group.

<h3> I now have to open up the root shell in terminal </h3>

The command is 

> sudo -i
<br /
> To confirm I'm in the root, I will use the ID command

<img src="https://i.imgur.com/MbtP4oy.png" height="80%" width="80%" alt="Agent Group created"/>
<br /

It should look like this after finishing. Now I will copy and paste the command to install the agent.

After you enter the command, the Nessus agent should begin installing. It will look like this

<img src="https://i.imgur.com/Kd5Qq0T.png" height="80%" width="80%" alt="Agent Group created"/>
<br /

<h3> After finishing, it looked like this with confirmation "The Nessus Agent is now linked to sensor.cloud.tenable.com </h3>
<br /

<img src="https://i.imgur.com/Kd5Qq0T.png" height="80%" width="80%" alt="Agent Group created"/>
<br /

<h2> Now it's time to create the file name. I named  the trigger file "goub.txt" to begin the scan. </h2>
<br /

The command for this is:

> touch /opt/nessus_agent/var/nessus/triggers/goub.txt

As you can see below, the file is now in the trigger folder. I will keep checking until the file disappears. This will confirm that the scan has begun.

<img src="https://i.imgur.com/hOQD3lt.png" height="80%" width="80%" alt="Agent Group created"/>
<br /

To check the status, I can also use the command below

> sudo systemctl status nessusagent.service

<h2> Now I just have to wait for the scan to commence and wait for the results. </h2>
