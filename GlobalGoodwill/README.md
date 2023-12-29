# Welcome to Global Goodwill

🥳 Today is your first day as a Junior Security Operations Center (SOC) Analyst with our company. Your primary job responsibility is to defend Global Goodwill and our employees from malicious cyber actors.

![243223309-cdd08773-1d97-4a99-a259-f9acefaea020](https://github.com/KC7-Foundation/kc7_data/assets/9474932/d5bbf3c1-bae6-407a-9ef6-4acf037b3940)

Global Goodwill is a not-for-profit focused on poverty alleviation, emergency aid and advocacy work all over the world. Operating with a widespread reach, the organization is committed to creating positive change and uplifting regions facing socio-economic challenges. Global Goodwill's core values center around integrity, collaboration, and empowerment - aiming to have a lasting impact on the lives of communities in need and allow them to achieve independence. 

Global Goodwill has a series of key partners who help us to extend our charitable efforts across the world:

| Partner Name | Relationship |
| ------------ | ----------- |
| Sun & Sand Trading (sunandsandtrading.com) | Leading distributor of high-quality beach products and accessories. Collaborates closely with Global Goodwill to provide a diverse range of merchandise, ensuring inventory meets the demands and preferences of beachgoers. |
| Beach Supply Co (beachsupplyco.com) | Specialized online retailer focused on providing beach essentials and supplies. Strategic partnership with Global Goodwill enhances online presence and expands customer base. Complements Global Goodwill's brick-and-mortar stores, creating a seamless omnichannel shopping experience. |
| Surf Paradise (surfparadise.com) | Popular online platform dedicated to surfing and beach culture. Partnership with Global Goodwill leverages their extensive audience of surf enthusiasts and beach lovers, enhancing brand visibility and driving traffic to stores and online channels. Provides valuable industry insights and trends. |
| Beach Lifestyle (beachlifestyle.com) | Lifestyle media and content platform focused on beach living, travel, and fashion. Partnership with Global Goodwill taps into an engaged community of beach enthusiasts, increasing brand exposure, and loyalty, and fostering a sense of community. Content and marketing expertise aligns with Global Goodwill's values. |


Global Goodwill has been focused on supporting civilians who have been displaced by recent global conflicts. However, this has made them a target for various cyber attackers so we need some extra help to keep the organization safe!

To try and keep the charity safe, Global Goodwill collects log data about the activity performed on the corporate network. These security audit logs are stored in Azure Data Explorer (ADX) - a data storage service in Azure (Microsoft’s cloud). You will use the Kusto Query Language (KQL) to parse through various types of security logs. By analyzing these logs, you can help us determine whether we’re being targeted by malicious actors.

You can find full documentation on ADX here: [https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer)

### Objectives

🧠 By the end of your first day on the job, you should be able to:

-   Use Azure Data Explorer and Kusto Query Language
-   Use multiple data sets to answer targeted questions 
-   Investigate cyber activity in logs including: email, web traffic, and server logs 
-   Use multiple techniques to track the activity of APTs (Advanced Persistent Threats) 
-   Use third-party data sets to discover things about your attackers
-   Make recommendations on what actions a company can take to protect themselves


**The attackers have gotten a head start, so let's not waste any more time... let's get to work!**


### Legend

> 🎯Key Point – Occasionally, you will see a dart emoji with a “key point.” These signal explanations of certain concepts that may enhance your understanding of key cybersecurity ideas that are demonstrated in the game. 

> 🤔Question – “Thinking” emojis represent questions that will enable you to demonstrate mastery of the concepts at hand. You can earn points by entering your responses to questions in the scoring portal available at kc7cyber.com/scoreboard.

> 🤫 Hint – “Whisper” emojis represent in-game hints. These hints will guide you in the right direction in answering some of the questions.

## Section 1: The Walkthrough 

#### Getting Set Up in Azure Data Explorer (ADX)

ADX is the primary tool used in the Global Goodwill SOC for data exploration and analysis. The great thing about ADX is that it is used by cyber analysts at many of the smallest and largest organizations in the world. 

Let’s get you logged in and started with ADX:

On the left sidebar, you’ll see a button that says **Query Data (ADX)**. Click this and it will redirect you to ADX! (Note: You’ll probably be asked to log in with a Microsoft account. You can use an existing personal or organization-issued Microsoft account, or create a new one for free.)


Once you login, you should see a cluster called  _“kc7cyber.eastus2”_   has already been added to your account.

![image](https://github.com/KC7-Foundation/kc7_data/assets/129029167/33983641-1dab-4c4d-8dac-0b65532d3b4f)

Data in ADX is organized in a hierarchical structure which consists of **clusters**, **databases**, and **tables**.

![image](https://github.com/KC7-Foundation/kc7_data/assets/129029167/08cfad5e-f136-4e72-a87b-514d6886b484)

All of Global Goodwill's security logs are stored in a single database – the Global Goodwill database. 

  2. Select your database.  
	- Expand the dropdown arrow next to the GlobalGoodwill database.
	- Click on the **GlobalGoodwill** database. Once you’ve done this, you should see the database highlighted- this means you’ve selected the database and are ready to query the tables inside.

Note: It’s very important that you use the Global Goodwill database for all questions while you’re investigating activity at Global Goodwill! If you choose the wrong database, you won’t be able to answer questions correctly.

The big space to the right of your cluster list is the _query workspace_. That’s where you’ll actually write the queries used to interact with our log data.

![image](https://github.com/KC7-Foundation/kc7_data/assets/129029167/648ed512-51a3-4c46-a298-1e1d14514b91)

Currently, you’ll see there’s a message there welcoming you to Global Goodwill! Click the blue Run button above the query workspace to run your first query! Once you’ve done that, you can erase the welcome message by highlighting it and pressing backspace or delete on your keyboard.

Okay, enough introductions… let’s get your hands on the data.

#### First Look at the data... 

The **GlobalGoodwill** database contains nine tables. Tables contain many rows of similar data. For security logs, a single row typically represents a single thing done by an employee or a device on the network at a particular time.

We currently have nine types of log data. As you’ll see in ADX, each log type corresponds to a table that exists in the **GlobalGoodwill** database:

| **Table Name** | **Description** | 
| ----------- | ----------- |
| AuthenticationEvents | Records successful and failed logins to devices on the company network. This includes logins to the company’s mail server.|
| Email | Records emails sent and received by employees|
| Employees | Contains information about the company’s employees| 
| FileCreationEvents | Records files stored on employee’s devices|
| InboundBrowsing| Records inbound network events including browsing activity from the Internet to devices within the company network|
| OutboundBrowsing | Records outbound network events including browsing activity from within the company network out to the Internet|
| PassiveDns (External) | Records IP-domain resolutions |
| ProcessEvents | Records processes created on employee’s devices |
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


The Employees table contains information about all the employees in our organization. In this case, we can see that the organization is named “Global Goodwill” and the domain is “castleandsand.com”.

> 1. 🤔 Try it for yourself! Do a **take** 10 on all the other tables to see what kind of data they contain.

Make sure you record your answer to all the questions from KQL 101 in the scoreboard at kc7cyber.com/scoreboard

You can easily write multiple queries in the same workspace tab. To do this, make sure to separate each query by an empty line. Notice below how we have separated the queries for the **Employees**, **Email**, and **OutboundBrowsing** tables by adding empty lines between them.

```sql
Email
| take 10

Employees
| take 10

OutboundBrowsing
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
  
We can use the **where** operator in KQL to apply filters to a particular field. For example, we can find all the employees with the name “Sam” by filtering on the name column in the Employees table.

**where** statements are written using a particular structure. Use this helpful chart below to understand how to structure a **where** statement.

| **where** | **field** | **operator** | **"value"** |
| ----------- | ----------- | ----------- | ----------- |
| where | name | has | "Sam" |

```sql
Employees
| where name has "Sam"
```

The **has** operator is useful here because we’re looking for only a partial match. If we wanted to look for an employee with a specific first and last name (an exact match), we’d use the == operator:

```sql
Employees
| where name == "Samantha Noe"
```

> 3. 🤔Each employee at Global Goodwill is assigned an IP address. Which employee has the IP address: “192.168.0.106”?

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


While performing their day-to-day tasks, Global Goodwill employees send and receive emails. A record of each of these emails is stored in the **Email** table.

> 🎯**Key Point – User Privacy and Metadata**: As you can imagine, some emails are highly sensitive. Instead of storing the entire contents of every email sent and received within the company in a database that can be easily accessed by security analysts, we only capture email metadata. 

Email metadata includes information like: the time the email was sent, the sender, the recipient, the subject line, and any links the email may contain. Storing only email metadata, rather than entire contents, helps protect the privacy of our employees, while also ensuring that our security analysts can keep us safe. Sometimes even metadata can reveal sensitive information, so it’s important that you don’t talk about log data with other employees outside the SOC.


We can find information about the emails sent or received by a user by looking for their email address in the sender and recipient fields of the **Email** table. For example, we can use the following query to see all the emails sent by “Samantha Noe”:

```sql

Email
| where sender == "samantha_noe@globalgoodwill.org"

```

> 4. 🤔How many emails did Karlyn Burgin receive?

**Easy as 1, 2, 3… Compound Queries and the distinct Operator**

We can use the **distinct** operator to find unique values in a particular column. We can use the following query to determine how many of the organization’s users sent emails.

```sql
Email
| where sender has "globalgoodwill.org"
| distinct sender
| count
```

This is our first time using a multi-line query with multiple operators, so let’s break it down:

In line 2, we take the Email table and filter the data down to find only those rows with “castleandsand.com” in the sender column.

In line 3, we add another pipe character ( | ) and use the distinct operator to find all the unique senders. Here, we aren’t finding the unique senders for all of the email senders, but only the unique senders that are left after we apply the filter looking for rows with “castleandsand.com” in the sender column.

Finally, in line 4, we add another pipe character ( | ) and then use the count operator to count the results of lines 1-3 of the query.

> 5. 🤔 How many distinct senders were seen in the email logs from sunandsandtrading.com?

**Tracking Down a Click: OutboundBrowsing Data**

When employees at Global Goodwill browse to a website from within the corporate network, that browsing activity is logged. This is stored in the **OutboundBrowsing** table, which contains records of the websites browsed by each user in the company. Whenever someone visits a website, a record of it is stored in the table. However, the user’s name is not stored in the table, only their IP address is recorded. There is a 1:1 relationship between users and their assigned IP addresses, so we can reference the **Employees** table to figure out who browsed a particular website. When a user visits a site, sometimes data from a lot of other sources are loaded as well. For example, images, assets, and other content may be hosted on content delivery network (CDN), which is used to deliver and load content quickly on a website. Sometimes, advertisements will also load from a particular website as well.

If we want to figure out what websites Rhonda Williams visited, we can find their IP address from the Employees table.

```sql
Employees
| where name == "Rhonda Williams"
```

The query above tells us their IP address is “192.168.0.106”. We can take their IP address and look in the **OutboundNetworkEvents** table to determine what websites they visited.

```sql
OutboundNetworkEvents
| where src_ip == "192.168.0.106"
```

> 6. 🤔 How many unique websites did “Rhonda Williams” visit?


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
| where name has "Sam"
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
let sam_ips = 
Employees
| where name has "Sam"
| distinct ip_addr;
OutboundNetworkEvents
| where src_ip in (sam_ips)
```

On the left of the let statement is the variable name (“james_ips” in this case). The variable name can be whatever we want, but it is helpful to make it something meaningful that can help us remember what values it is storing.

```sql  hl_lines="1"
let sam_ips = 
Employees
| where name has "Sam"
| distinct ip_addr;
OutboundNetworkEvents
| where src_ip in (sam_ips)
```
On the right side of the let statement in the expression you are storing. In this case, we use the **distinct** operator to select values from only one column – so they are stored in an array – or list of values.

```sql  hl_lines="2 3 4"
let james_ips = 
Employees
| where name has "Sam"
| distinct ip_addr;
OutboundNetworkEvents
| where src_ip in (sam_ips)
```

The **let** statement is concluded by a semi-colon.

```sql  hl_lines="4"
let sam_ips = 
Employees
| where name has "Sam"
| distinct ip_addr;
OutboundNetworkEvents
| where src_ip in (sam_ips)
```

After we store the value of a query into a variable using the **let** statement, we can refer to it as many times as we like in the rest of the query. The stored query does not show any output. Remember, however, that your KQL query must have a tabular statement – which means that you must have another query following your **let** statement.

> 9. 🤔 How many unique URLs were browsed by employees named “James”?

> 🎯**Key Point – Pivoting:** Part of being a great cyber analyst is learning how to use multiple data sources to tell a more complete story of what an attacker has done. We call this “pivoting.” We pivot by taking one known piece of data in one dataset and looking in a different dataset to learn something we didn’t already know. You practiced this here when we started in one dataset – the Employees table – and used knowledge from there to find related data in another source – OutboundNetworkEvents.


## Section 2: Start Hunting! 

You’ve finished your training and you’re ready to get to work protecting Global Goodwill. 

Work with your team to complete as many challenge questions from the remaining sections in the scoreboard as possible! The goal is to score as many points as you can. There are a lot of questions (the attackers have been busy), so you probably won’t be able to answer them all. Just do as many as you can!

As you answer the questions, we will take you on a journey exploring the data and discovering what actions the adversaries have taken. However, you should remember that this is only one of many paths you can take through the data.  As you go, don’t forget to pay attention to the details along the way. What patterns do the attackers exhibit that could help you track them better? Do they like to use certain words, themes? Or do they make mistakes? Keeping track of these patterns will help you build the full picture of what happened. 


## Resources

Understanding KQL operators: [https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators)


