# Welcome to Castle&Sand

🥳 Today is your first day as a Junior Security Operations Center (SOC) Analyst with our company. Your primary job responsibility is to defend Castle&Sand and our employees from malicious cyber actors.

![243223309-cdd08773-1d97-4a99-a259-f9acefaea020](https://github.com/KC7-Foundation/kc7_data/assets/9474932/d5bbf3c1-bae6-407a-9ef6-4acf037b3940)


Castle&Sand is a renowned company in the beach accessories and gear industry, operating a vast network of stores across multiple locations. With a comprehensive range of products and a dedicated team of knowledgeable staff, Castle&Sand offers customers an exceptional shopping experience. The company's commitment to research and development ensures that they stay ahead of industry trends, providing high-quality and innovative beach accessories and gear. Castle&Sand's focus on customer satisfaction, welcoming store environments, and continuous improvement has solidified its position as a leader in the industry, making it the go-to destination for all beachgoers' needs.

Castle&Sand has a series of key partners who contribute to the success of our business:

| Partner Name | Relationship |
| ------------ | ----------- |
| Sun & Sand Trading (sunandsandtrading.com) | Leading distributor of high-quality beach products and accessories. Collaborates closely with Castle&Sand to provide a diverse range of merchandise, ensuring inventory meets the demands and preferences of beachgoers. |
| Beach Supply Co (beachsupplyco.com) | Specialized online retailer focused on providing beach essentials and supplies. Strategic partnership with Castle&Sand enhances online presence and expands customer base. Complements Castle&Sand's brick-and-mortar stores, creating a seamless omnichannel shopping experience. |
| Surf Paradise (surfparadise.com) | Popular online platform dedicated to surfing and beach culture. Partnership with Castle&Sand leverages their extensive audience of surf enthusiasts and beach lovers, enhancing brand visibility and driving traffic to stores and online channels. Provides valuable industry insights and trends. |
| Beach Lifestyle (beachlifestyle.com) | Lifestyle media and content platform focused on beach living, travel, and fashion. Partnership with Castle&Sand taps into engaged community of beach enthusiasts, increasing brand exposure, loyalty, and fostering a sense of community. Content and marketing expertise aligns with Castle&Sand's values. |


Castle&Sand has been laser-focused on broadening our customer base. Recently, though, we have been getting a lot of attention. All this extra attention has brought along some unwelcome visitors- cyber attackers! That’s why we’ve hired you! To help keep us safe!

Like all good companies, Castle&Sand collects log data about the activity performed on the corporate network. These security audit logs are stored in Azure Data Explorer (ADX) - a data storage service in Azure (Microsoft’s cloud). You will use the Kusto Query Language (KQL) to parse through various types of security logs. By analysing these logs, you can help us determine whether we’re being targeted by malicious actors.

You can find full documentation on ADX here: [https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer)

### Objectives

🧠 By the end of your first day on the job, you should be able to:

-   Use Azure Data Explorer and Kusto Query Language
-   Use multiple data sets to answer targeted questions 
-   Investigate cyber activity in logs including: email, web traffic, and server logs 
-   Use multiple techniques to track the activity of APTs (Advanced Persistent Threats) 
-   Use third party data sets to discover things about your attackers
-   Make recommendations on what actions a company can take to protect themselves


**The attackers have gotten a head start, so let's not waste any more time... let's get to work!**


You can find all the links you need here: [kc7cyber.com/castle-and-sand](http://kc7cyber.com/castle-and-sand)

### Legend

> 🎯Key Point – Occasionally, you will see a dart emoji with a “key point.” These signal explanations of certain concepts that may enhance your understanding of key cybersecurity ideas that are demonstrated in the game. 

> 🤔Question – “Thinking” emojis represent questions that will enable you to demonstrate mastery of the concepts at hand. You can earn points by entering your responses to questions in the scoring portal available at kc7cyber.com/scoreboard.

> 🤫 Hint – “Whisper” emojis represent in-game hints. These hints will guide you in the right direction in answering some of the questions.

## Section 1: The Walkthrough 

#### Getting Set Up in Azure Data Explorer (ADX)

ADX is the primary tool used in the Castle&Sand SOC for data exploration and analysis. The great thing about ADX is that it is used by cyber analysts at many of the smallest and largest organizations in the world. 

Let’s get you logged in and started with ADX:

1. Go to the Castle&Sand training module at [kc7cyber.com/castle-and-sand](http://kc7cyber.com/castle-and-sand)

On the training module page, you’ll see a button that says **Get the Data**. Click this and it will redirect you to ADX! (Note: You’ll probably be asked to login with a Microsoft account. You can use an existing personal or organization-issued Microsoft account, or create a new one for free.)

![](https://lh5.googleusercontent.com/XVuNwIMKtT5WtYaxLkPRnuezZQfpWBDNa8naLYn31uFLDyd2A8NxrXYK86Vf9wEkdwuMM3Sp0PDdS9T3LXkeWJLYd2w_OVr39dB17_T-ItxDycIdURHQ74vMNQ-zMHVWJIsXeeuFjhoV)

Once you login, you should see a cluster called  _“kc7cyber.eastus2”_   has already been added to your account.

![image](https://github.com/KC7-Foundation/kc7_data/assets/129029167/33983641-1dab-4c4d-8dac-0b65532d3b4f)

Data in ADX is organized in a hierarchical structure which consists of **clusters**, **databases**, and **tables**.

![image](https://github.com/KC7-Foundation/kc7_data/assets/129029167/08cfad5e-f136-4e72-a87b-514d6886b484)

All of Castle&Sand’s security logs are stored in a single database – the CastleSand database. 

  2. Select your database.  
	- Expand the dropdown arrow next to the Castle&Sand database.
	- Click on the **CastleSand** database. Once you’ve done this, you should see the database highlighted- this means you’ve selected the database and are ready to query the tables inside.

Note: It’s very important that you use the Castle&Sand database for all questions while you’re investigating activity at Castle&Sand! If you choose the wrong database, you won’t be able to answer questions correctly.

The big space to the right of your cluster list is the _query workspace_. That’s where you’ll actually write the queries used to interact with our log data.

![image](https://github.com/KC7-Foundation/kc7_data/assets/129029167/648ed512-51a3-4c46-a298-1e1d14514b91)

Currently, you’ll see there’s a message there welcoming you to Castle&Sand! Click the blue Run button above the query workspace to run your first query! Once you’ve done that, you can erase the welcome message by highlighting it and pressing backspace or delete on your keyboard.

Okay, enough introductions… let’s get your hands on the data.

#### First Look at the data... 

The **CastleSand** database contains nine tables. Tables contain many rows of similar data. For security logs, a single row typically represents a single thing done by an employee or a device on the network at a particular time.

We currently have nine types of log data. As you’ll see in ADX, each log type corresponds to a table that exists in the **CastleSand** database:

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


#### KQL 101 

Type the following query in the workspace to view the first rows in the **Employees** table. Press “run” or “shift + enter” to execute the query.

```sql
Employees
| take 10
```

This query has a few parts. Let’s take a moment to break each of them down:

![Diagram
Description automatically generated with low confidence](https://lh4.googleusercontent.com/y7YpmVbUlak4wMrI43nYzAqieamRnwiANLJzy8UsxHHaYJI5SlJVpTdz4EO47A-g0SVI77ehgZxHB13AZXLKKX02Yr40VZmFsb3blZgHCojeD-vS34SS24yWWqK6rnduGNdhmnXlCAQAMWETrm7_hg)

| **Query Component** | **Description** | 
| ----------- | ----------- |
| Table Name | The table name specifies which table/data source the query will pull data from. All queries must start with a table.|
| Pipe character (&#124;) | The pipe character indicates the start of a new part of the query. A pipe will be added automatically after typing a table name and pressing enter. You can also add a pipe character manually by holding shift and pressing the backslash (&#92;) key. That’s the one just below the backspace key.

The **take** operator is a powerful tool you can use to explore rows in a table, and therefore better understand what kinds of data are stored there.

> 🎯**Key Point – What to do when you don’t know what to do**: Whenever you are faced with an unfamiliar database table, the first thing you should do is sample its rows using the **take** operator. That way, you know what fields are available for you to query and you can guess what type of information you might extract from the data source.


The Employees table contains information about all the employees in our organization. In this case, we can see that the organization is named “Castle&Sand” and the domain is “castleandsand.com”.

> 1. 🤔 Try it for yourself! Do a **take** 10 on all the other tables to see what kind of data they contain.

Make sure you record your answer to all the questions from KQL 101 in the scoreboard at kc7cyber.com/scoreboard

You can easily write multiple queries in the same workspace tab. To do this, make sure to separate each query by an empty line. Notice below how we have separated the queries for the **Employees**, **Email**, and **OutboundNetworkEvents** tables by adding empty lines between them.

```sql
Email
| take 10

Employees
| take 10

OutboundNetworkEvents
| take 10
```

When you have multiple queries, it’s important to tell ADX which query you want to run. To choose a query, just click on any line that is part of that query. 

**Finding Out “How Many”: The count Operator**

We can use **count** to see how many rows are in a table. This tells us how much data is stored there.

```sql
Employees
| count
```

> 2.  🤔How many employees are in the company?

**Filtering Data With the _where_ Operator**

So far, we’ve run queries that look at the entire contents of the table. Often in cybersecurity analysis, we only want to look at data that meets a set of conditions or criteria. To accomplish this, we apply filters to specific columns.
  
We can use the **where** operator in KQL to apply filters to a particular field. For example, we can find all the employees with the name “Jason” by filtering on the name column in the Employees table.

**where** statements are written using a particular structure. Use this helpful chart below to understand how to structure a **where** statement.

| **where** | **field** | **operator** | **"value"** |
| ----------- | ----------- | ----------- | ----------- |
| where | name | has | "Jason" |

```sql
Employees
| where name has "Jason"
```

The **has** operator is useful here because we’re looking for only a partial match. If we wanted to look for an employee with a specific first and last name (an exact match), we’d use the == operator:

```sql
Employees
| where name == "Jason Goldsmith"
```

> 3. 🤔Each employee at Castle&Sand is assigned an IP address. Which employee has the IP address: “10.10.2.1”?

Here are some additional operators we like to use:

| Operator | Description | Case-Sensitive | Example (yields true) |
|---|---|---|---|
| == | Equals | Yes | "aBc" == "aBc" |
| != | Not equals | Yes | "abc" != "ABC" |
| =~ | Equals | No | "abc" =~ "ABC" |
| contains | Right-hand-side (RHS) occurs as a subsequence of left-hand-side (LHS) | No | "FabriKam" contains "BRik" |
| has | RHS is a whole term in LHS | No | "North America" has "america" |
| has_all | Same as has but works on all of the elements | No | "North and South America" has_all("south", "north") |
| has_any | Same as has but works on any of the elements | No | "North America" has_any("south", "north") |
| in | Equals to any of the elements | Yes | "abc" in ("123", "345", "abc") |


While performing their day-to-day tasks, Castle&Sand employees send and receive emails. A record of each of these emails is stored in the **Email** table.

> 🎯**Key Point – User Privacy and Metadata**: As you can imagine, some emails are highly sensitive. Instead of storing the entire contents of every email sent and received within the company in a database that can be easily accessed by security analysts, we only capture email metadata. 

Email metadata includes information like: the time the email was sent, the sender, the recipient, the subject line, and any links the email may contain. Storing only email metadata, rather than entire contents, helps protect the privacy of our employees, while also ensuring that our security analysts can keep us safe. Sometimes even metadata can reveal sensitive information, so it’s important that you don’t talk about log data with other employees outside the SOC.


We can find information about the emails sent or received by a user by looking for their email address in the sender and recipient fields of the **Email** table. For example, we can use the following query to see all the emails sent by “Jason Goldsmith”:

```sql

Email
| where sender == "jason_goldsmith@castleandsand.com"

```

> 4. 🤔How many emails did Jacqueline Henderson receive?

**Easy as 1, 2, 3… Compound Queries and the distinct Operator**

We can use the **distinct** operator to find unique values in a particular column. We can use the following query to determine how many of the organization’s users sent emails.

```sql
Email
| where sender has "castleandsand.com"
| distinct sender
| count
```

This is our first time using a multi-line query with multiple operators, so let’s break it down:

In line 2, we take the Email table and filter the data down to find only those rows with “castleandsand.com” in the sender column.

In line 3, we add another pipe character ( | ) and use the distinct operator to find all the unique senders. Here, we aren’t finding the unique senders for all of the email senders, but only the unique senders that are left after we apply the filter looking for rows with “castleandsand.com” in the sender column.

Finally, in line 4, we add another pipe character ( | ) and then use the count operator to count the results of lines 1-3 of the query.

> 5. 🤔 How many distinct senders were seen in the email logs from sunandsandtrading.com?

**Tracking Down a Click: OutboundNetworkEvents Data**

When employees at Castle&Sand browse to a website from within the corporate network, that browsing activity is logged. This is stored in the **OutboundNetworkEvents** table, which contains records of the websites browsed by each user in the company. Whenever someone visits a website, a record of it is stored in the table. However, the user’s name is not stored in the table, only their IP address is recorded. There is a 1:1 relationship between users and their assigned IP addresses, so we can reference the **Employees** table to figure out who browsed a particular website. When a user visits a site, sometimes data from a lot of other sources are loaded as well. For example, images, assets, and other content may be hosted on content delivery network (CDN), which is used to deliver and load content quickly on a website. Sometimes, advertisements will also load from a particular website as well.

If we want to figure out what websites Cristin Genao visited, we can find their IP address from the Employees table.

```sql
Employees
| where name == "Cristin Genao"
```

The query above tells us their IP address is “10.10.0.84”. We can take their IP address and look in the **OutboundNetworkEvents** table to determine what websites they visited.

```sql
OutboundNetworkEvents
| where src_ip == "10.10.0.84"
```

> 6. 🤔 How many unique websites did “Cristin Genao” visit?


**What’s in a Name? All about Passive DNS Data**

Although domain names like “google.com” are easy for humans to remember, computers don’t know how to handle them. So, they convert them to machine readable IP addresses. Just like your home address tells your friends how to find your house or apartment, an IP address tells your computer where to find a page or service hosted on the internet.

> 🎯**Key Point – Practice Good OPSEC**: If we want to find out which IP address a particular domain resolves to, we could just browse to it. But, if the domain is a malicious one, you could download malicious files to your corporate analysis system or tip off the attackers that you know about their infrastructure. As cybersecurity analysts, we must follow procedures and safeguards that protect our ability to track threats. These practices are generally called operational security, or OPSEC.

To eliminate the need to actively resolve (that is- directly browse to or interact with a domain to find it’s related IP address) every domain we’re interested in, we can rely on passive DNS data. Passive DNS data allows us to safely explore domain-to-IP relationships, so we can answer questions like:

-   *Which IP address does this domain resolve to?*
-   *Which domains are hosted on this IP address?*
-   *How many other IPs have this domain resolved to?*

These domain-to-IP relationships are stored in our **PassiveDns** table.

> 7. 🤔 How many domains in the PassiveDns records contain the word “shark”? (hint: use the contains operator instead of has. If you get stuck, do a take 10 on the table to see what fields are available.)
> 8. 🤔 What IPs did the domain “sharkfin.com” resolve to (enter any one of them)?

**🤯Let statements – making your life a bit easier:**

Sometimes we need to use the output of one query as the input for a second query.  The first way we can do this is by manually typing the results into the next query.

For example, what if we want to look at all the web browsing activity from employees named “James”?

First, you would need to go into the **Employees** table and find the IP addresses used by these employees.


```sql
Employees
| where name has "James"
```

![image](https://github.com/KC7-Foundation/kc7_data/assets/129029167/9565f7fc-9008-45fa-9439-9a35a910f5e1)

Then, you could manually copy and paste these IPs into a query against the **OutboundNetworkEvents** table. Note that we can use the in operator to choose all rows that have a value matching any value from a list of possible values. In other words, the == (comparison) operator looks for an exact match, while the in operator checks for any values from the list

```sql 
OutboundNetworkEvents
| where src-ip in ("10.10.3.102",
	"10.10.3.48",
	"10.10.4.37",
	"10.10.2.143",
	"10.10.2.143",
	"10.10.3.155",
	"10.10.5.180",
	"10.10.1.178",
	"10.10.5.67",
	"10.10.1.221")
```

Although this is a valid way to get the information you need, it may not be as elegant (or timely) if you had 100 or even 1000 employees named “James.”

We can accomplish this in a more elegant way by using a [let statement,](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/letstatement) which allows us to assign a name to an expression or a function. We can use a let statement here to save and give a name to the results of the first query so that the values can be re-used later. That means we don’t have to manually type or copy and paste the results repeatedly.

```sql 
let james_ips = 
Employees
| where name has "James"
| distinct ip_addr;
OutboundNetworkEvents
| where src_ip in (james_ips)
```

On the left of the let statement is the variable name (“james_ips” in this case). The variable name can be whatever we want, but it is helpful to make it something meaningful that can help us remember what values it is storing.

```sql  hl_lines="1"
let james_ips = 
Employees
| where name has "James"
| distinct ip_addr;
OutboundNetworkEvents
| where src_ip in (james_ips)
```
On the right side of the let statement in the expression you are storing. In this case, we use the **distinct** operator to select values from only one column – so they are stored in an array – or list of values.

```sql  hl_lines="2 3 4"
let james_ips = 
Employees
| where name has "James"
| distinct ip_addr;
OutboundNetworkEvents
| where src_ip in (james_ips)
```

The **let** statement is concluded by a semi-colon.

```sql  hl_lines="4"
let james_ips = 
Employees
| where name has "James"
| distinct ip_addr;
OutboundNetworkEvents
| where src_ip in (james_ips)
```

After we store the value of a query into a variable using the **let** statement, we can refer to it as many times as we like in the rest of the query. The stored query does not show any output. Remember, however, that your KQL query must have a tabular statement – which means that you must have another query following your **let** statement.

> 9. 🤔 How many unique URLs were browsed by employees named “Karen”?

> 🎯**Key Point – Pivoting:** Part of being a great cyber analyst is learning how to use multiple data sources to tell a more complete story of what an attacker has done. We call this “pivoting.” We pivot by taking one known piece of data in one dataset and looking in a different dataset to learn something we didn’t already know. You practiced this here when we started in one dataset – the Employees table – and used knowledge from there to find related data in another source – OutboundNetworkEvents.


## Section 2: Start Hunting! 

You’ve finished your training and you’re ready to get to work protecting Castle&Sand. 

Work with your team to complete as many challenge questions from the remaining sections in the scoreboard as possible! The goal is to score as many points as you can. There are a lot of questions (the attackers have been busy), so you probably won’t be able to answer them all. Just do as many as you can!

As you answer the questions, we will take you on a journey exploring the data and discovering what actions the adversaries have taken. However, you should remember that this is only one of many paths you can take through the data.  As you go, don’t forget to pay attention to the details along the way. What patterns do the attackers exhibit that could help you track them better? Do they like to use certain words, themes? Or do they make mistakes? Keeping track of these patterns will help you build the full picture of what happened. 

Use the provided [Actor Preview](https://docs.google.com/document/d/1rZR4eVG886oPziG-5nGeQ5kN_q5Bpq0m/edit?usp=sharing&ouid=105873493764084037775&rtpof=true&sd=true) document to keep track of what you know about the attacker. Building a good profile, timelining the attacker’s activity, and forming a list of indicators of compromise (IOCs) will help you keep track of the attacker. KC7 models some of the techniques used by these attackers from real world threat actors, so it may be a helpful resource for you in the future when you are investigating a real security incident. 

Now, get out there and keep us safe! The whole company is counting on you. No pressure😊.

## Nice to Know

Here are some things that might be helpful to know as we explore the CastleSand Database:

#### Business hours

We operate our business from 9am to 5pm, 7 days a week. We should roughly expect to see all legitimate business activity within that timeframe (with some exceptions, of course). Let's be vigilant about any activity that occurs on the network while everyone sleeps ;)

#### Pay attention to the IT Workers

Our IT staff at Castle&Sand are granted elevated privileges on machines across the company in order to fulfill their duties. As a result, it can be concerning when their machines get compromised. We, at Castle&Sand, are a security-aware organization, so we do not grant local administrative privileges to users on their computers. However, each laptop and desktop at Castle&Sand is provisioned with a separate local administrative account - which looks something like "marketing_local_admin." These administrator accounts are provisioned with the same username and password per department. For example, all employees in the Marketing department have a local administrator account with the username "marketing_local_admin" and the same password.

#### Castle&Sand uses Active Directory

We utilize Active Directory, a directory service developed by Microsoft for Windows networks. It provides us with a centralized and standardized system for managing and organizing network resources, such as user accounts, computers, groups, and other network objects.

Unfortunately, compromising the Active Directory Domain Controller would give an adversary credentials for all accounts on our network and enable them to cause severe harm.

#### Alerts and Alert Fatigue

In the CastleSand database, we have access to a **SecurityAlerts** table that shows suspicious security events that have occurred on the Castle&Sand network. This can be a great place to begin searching for malicious activity. However, we need to be aware that some of these alerts are false positives, which means they may indicate non-malicious activity. Let's be careful not to waste too much time chasing after these.


## Mitre ATT&CK Techniques

The following Mitre ATT&CK techniques were used my by the actors in the observed intrusions

### Reconnaissance
- **T1594** - Search Victim-Owned Websites:
Adversaries may search websites owned by the victim for information that can be used during targeting. Victim-owned websites may contain a variety of details, including names of departments/divisions, physical locations, and data about key employees such as names, roles, and contact info (ex: Email Addresses). These sites may also have details highlighting business operations and relationships.

### Resource Development
- **T1584** - Compromise Infrastructure:
Adversaries may compromise third-party infrastructure that can be used during targeting. Infrastructure solutions include physical or cloud servers, domains, and third-party web and DNS services. Instead of buying, leasing, or renting infrastructure an adversary may compromise infrastructure and use it during other phases of the adversary lifecycle.

### Initial Access
- **T1189** - Drive-by Compromise:
Adversaries may gain access to a system through a user visiting a website over the normal course of browsing. With this technique, the user's web browser is typically targeted for exploitation, but adversaries may also use compromised websites for non-exploitation behavior such as acquiring Application Access Token.

- **T1566** - Phishing:
Adversaries may send phishing messages to gain access to victim systems. All forms of phishing are electronically delivered social engineering. Phishing can be targeted, known as spearphishing. In spearphishing, a specific individual, company, or industry will be targeted by the adversary. More generally, adversaries can conduct non-targeted phishing, such as in mass malware spam campaigns.

- **T1195** - Supply Chain Compromise:
Adversaries may manipulate products or product delivery mechanisms prior to receipt by a final consumer for the purpose of data or system compromise.

### Execution
- **T1047** - Windows Management Instrumentation:
Adversaries may abuse Windows Management Instrumentation (WMI) to execute malicious commands and payloads.

- **T1053** - Scheduled Task/Job:
Adversaries may abuse task scheduling functionality to facilitate initial or recurring execution of malicious code.

### Persistence
- **T1574** - Hijack Execution Flow:
Adversaries may execute their own malicious payloads by hijacking the way operating systems run programs.

- **T1078** - Valid Accounts:
Adversaries may obtain and abuse credentials of existing accounts as a means of gaining Initial Access, Persistence, Privilege Escalation, or Defense Evasion.

### Privilege Escalation
- **T1078** - Valid Accounts:
Adversaries may obtain and abuse credentials of existing accounts as a means of gaining Initial Access, Persistence, Privilege Escalation, or Defense Evasion.

- **T1484** - Domain Policy Modification:
Adversaries may modify the configuration settings of a domain to evade defenses and/or escalate privileges in domain environments.

### Defense Evasion
- **T1070** - Indicator Removal:
Adversaries may delete or modify artifacts generated within systems to remove evidence of their presence or hinder defenses.

- **T1574** - Hijack Execution Flow:
Adversaries may execute their own malicious payloads by hijacking the way operating systems run programs.

- **T1562** - Impair Defenses:
Adversaries may maliciously modify components of a victim environment in order to hinder or disable defensive mechanisms.

### Credential Access
- **T1110** - Brute Force:
Adversaries may use brute force techniques to gain access to accounts when passwords are unknown or when password hashes are obtained.

- **T1003** - OS Credential Dumping:
Adversaries may attempt to dump credentials


## Glossary

**Watering hole:** A type of attack where a hacker compromises a website that is frequently visited by a specific group of users, such as employees of a certain organization, and then infects their devices with malware when they visit the site.

**Phishing:** A type of attack where a hacker sends an email or other message that appears to be from a legitimate source, such as a bank or a colleague, and tries to trick the recipient into clicking on a malicious link, opening an attachment, or providing sensitive information.

**Credential theft:** A type of attack where a hacker steals or obtains the username and password of a user or an administrator, and then uses them to access their accounts or systems.

**Password spray:** A type of attack where a hacker tries to guess the passwords of multiple accounts by using common or weak passwords, such as “password” or “123456”, instead of targeting one account with many password attempts.

**Reconnaissance:** The process of gathering information about a target system, network, organization, or user before launching an attack. This can include scanning for open ports, identifying vulnerabilities, mapping network topology, collecting email addresses, etc.

**Supply chain compromise:** A type of attack where a hacker infiltrates the software development process or distribution channel of a trusted vendor or partner and inserts malicious code into their products or services. This way, the hacker can compromise the customers or users who install or use those products or services.

**Malware:** A general term for any software that is designed to harm or disrupt a system, network, device, or data. Malware can include viruses, worms, trojans

**Command and control:** A type of server or network that is used by hackers to communicate with and control their malware or botnets on compromised systems, networks, or devices. Command and control servers can send commands, receive data, update malware, or launch attacks.

**Adversary in the middle:** A type of attack where a hacker intercepts and modifies the communication between two parties, such as a user and a website, without their knowledge. Adversary in the middle attacks can be used to steal or alter data, redirect traffic, inject malware, or impersonate either party.

**Top level domain:** The highest level of domain names in the Internet’s domain name system. Top level domains are the last part of a domain name after the dot, such as .com, .org, .edu, etc. Top level domains can indicate the type or purpose of a website or its geographic location.

## Resources

Understanding KQL operators: [https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators)


