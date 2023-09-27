# Welcome to Sunlands Aeronautics and Space Administration (SASA)

🥳 Today is your first day as a Junior Security Operations Center (SOC) Analyst with the most advanced space program in the world. Your primary job responsibility is to defend Sunlands Aeronautics and Space Administration (SASA) and our employees from malicious cyber actors.

![sasa](https://github.com/KC7-Foundation/kc7_data/assets/9474932/26fda1ff-c5c2-4c5d-9e90-f553375544fc)


### Introduction

Sunlands Aeronautics and Space Administration (SASA) is the esteemed government agency of the United Sunlands responsible for the civil space program, aeronautics research, and space research.

The International Space Summit took place on September 1, 2123 in the United Sunlands, where fellow space powers convened to discuss the development of new spaceports and other infrastructure to enable greater space exploration. The United Sunlands, an emerging economy, revolutionized its industries by harnessing solar power and recently developed cutting-edge solar powered space propulsion technology. The United Sunlands is entertaining offers for spaceport funding from two other countries: Luneria States and Astrella Republic.

However, the United Sunlands was targeted during this summit. The attacker exfiltrated sensitive, secret data about the United Sunlands' latest rocket technology as well as confidential, high-level communications about which country the United Sunlands would likely go with for the deal. A botnet is spreading an online influence campaign with the narrative that Luneria States was responsible for the attack.

Your mission, if you choose to accept it, is to investigate the attack and report your forensic attribution assessment to the United Sunlands president.

Sunlands Aeronautics and Space Administration (SASA) collects log data about the activity our employees perform on the organization's network. These security audit logs are stored in Azure Data Explorer (ADX) - a data storage service in Azure (Microsoft’s cloud). You will use the Kusto Query Language (KQL) to parse through various types of security logs. By analysing these logs, you can help us determine whether we’re being targeted by malicious actors.

You can find full documentation on ADX here: [https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer)

### Objectives

🧠 By the end of your first day on the job, you should be able to:

-   Use the Azure Data Explorer
-   Use multiple data sets to answer targeted questions 
-   Investigate cyber activity in logs including: email, web traffic, and server logs 
-   Use multiple techniques to track the activity of APTs (Advanced Persistent Threats) 
-   Use third party data sets to discover things about your attackers
-   Make recommendations on what actions a company can take to protect themselves


**The attackers have gotten a head start, so let's not waste any more time... let's get to work!**


You can find all the links you need here: [http://scoreboard.kc7cyber.com/modules/SASA](http://scoreboard.kc7cyber.com/modules/SASA)

#### Getting Set Up in Azure Data Explorer (ADX)

ADX is the primary tool used in the Sunlands Aeronautics and Space Administration (SASA) SOC for data exploration and analysis. The great thing about ADX is that it is used by cyber analysts at many of the smallest and largest organizations in the world. 

Let’s get you logged in and started with ADX:

On the left sidebar, you’ll see a button that says **Query Data (ADX)**. Click this and it will redirect you to ADX! (Note: You’ll probably be asked to login with a Microsoft account. You can use an existing personal or organization-issued Microsoft account, or create a new one for free.)

![image](https://github.com/KC7-Foundation/kc7_data/assets/9474932/aaab3ee6-95c6-4806-99f2-a2e9e6ada2bf)


Once you login, you should see a cluster called  _“kc7cyber.eastus2”_   has already been added to your account.

![image](https://github.com/KC7-Foundation/kc7_data/assets/144710889/cc798f5c-a6b9-4928-af4c-25b13d02a83c)


Data in ADX is organized in a hierarchical structure which consists of **clusters**, **databases**, and **tables**.

![image](https://github.com/KC7-Foundation/kc7_data/assets/144710889/c4114658-b8e3-4822-a0ea-951d577082d8)


All of Sunlands Aeronautics and Space Administration (SASA)’s security logs are stored in a single database – the **Sunlands** database. 

  2. Select your database.  
	- Expand the dropdown arrow next to the **Sunlands** database.
	- Click on the **Sunlands** database. Once you’ve done this, you should see the database highlighted- this means you’ve selected the database and are ready to query the tables inside.

Note: It’s very important that you use the **Sunlands** database for all questions while you’re investigating activity at SASA! If you choose the wrong database, you won’t be able to answer questions correctly.

The big space to the right of your cluster list is the _query workspace_. That’s where you’ll actually write the queries used to interact with our log data.

![image](https://github.com/KC7-Foundation/kc7_data/assets/144710889/2c15aae9-2334-46a5-b3f1-fb47fbe6c47f)

Currently, you’ll see there’s a message there welcoming you to Sunlands Aeronautics and Space Administration (SASA)! Click the blue Run button above the query workspace to run your first query! Once you’ve done that, you can erase the welcome message by highlighting it and pressing backspace or delete on your keyboard.

Okay, enough introductions… let’s get your hands on the data.

#### First Look at the data... 

The **Sunlands Aeronautics and Space Administration (SASA)** database contains nine tables. Tables contain many rows of similar data. For security logs, a single row typically represents a single thing done by an employee or a device on the network at a particular time.

We currently have nine types of log data. As you’ll see in ADX, each log type corresponds to a table that exists in the **Sunlands Aeronautics and Space Administration (SASA)** database:

| **Table Name** | **Description** | 
| ----------- | ----------- |
| Employees | Contains information about the company’s employees| 
| Email | Records emails sent and received by employees|
| InboundNetworkEvents | Records inbound network events including browsing activity from the Internet to devices within the company network|
| OutboundNetworkEvents | Records outbound network events including browsing activity from within the company network out to the Internet|
| AuthenticationEvents | Records successful and failed logins to devices on the company network. This includes logins to the company’s mail server.|
| FileCreationEvents | Records files stored on employee’s devices|
| ProcessEvents | Records processes created on employee’s devices |
| PassiveDns (External) | Records IP-domain resolutions |
| SecurityAlerts | Records security alerts from an employee’s device or the company’s email security system |

> 🎯**Key Point – Over the Horizon (OTH) data**: One of the tables listed above is not like the others – **PassiveDns**. Rather than being an internal security log, PassiveDns is a data source that we’ve purchased from a 3rd party vendor. Not all malicious cyber activity happens within our company network, so sometimes we depend on data from other sources to complete our investigations.


You’ll learn more about how to use each of these datasets in just a minute. First, let’s just run some queries so you can practice using KQL and ADX.



## Section 2: Start Hunting! 

You’ve finished your training and you’re ready to get to work protecting Sunlands Aeronautics and Space Administration (SASA). 

Work with your team to complete as many challenge questions from the remaining sections in the scoreboard as possible! The goal is to score as many points as you can. There are a lot of questions (the attackers have been busy), so you probably won’t be able to answer them all. Just do as many as you can!

As you answer the questions, we will take you on a journey exploring the data and discovering what actions the adversaries have taken. However, you should remember that this is only one of many paths you can take through the data.  As you go, don’t forget to pay attention to the details along the way. What patterns do the attackers exhibit that could help you track them better? Do they like to use certain words, themes? Or do they make mistakes? Keeping track of these patterns will help you build the full picture of what happened. 

Use the provided [Actor Preview](https://docs.google.com/document/d/1rZR4eVG886oPziG-5nGeQ5kN_q5Bpq0m/edit?usp=sharing&ouid=105873493764084037775&rtpof=true&sd=true) document to keep track of what you know about the attacker. Building a good profile, timelining the attacker’s activity, and forming a list of indicators of compromise (IOCs) will help you keep track of the attacker. KC7 models some of the techniques used by these attackers from real world threat actors, so it may be a helpful resource for you in the future when you are investigating a real security incident. 

Now, get out there and keep us safe! The whole company is counting on you. No pressure😊.


## Resources

Understanding KQL operators: [https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators)
