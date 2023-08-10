⚠️  Legal Disclaimer:
The companies, organizations, domain names, email addresses, people, places, and events depicted in the KC7 Cyber Game data sets and other materials created by the KC7 Foundation are fictitious.  No association with any real company, organization, domain name, email address, person, place, or event is intended or should be inferred._

# Welcome to World Domination Nation Consulting

🥳 Today is your first day as a Junior Security Operations Center (SOC) Analyst with our company. Your primary job responsibility is to defend WDN Consulting and our employees from malicious cyber actors.

[![domination_nation](https://github.com/KC7-Foundation/kc7_data/assets/9474932/ae93a444-18d4-4550-9bd7-c98568fe626b)
](https://res.cloudinary.com/dt8l5w5wi/image/upload/v1691654392/domination_nation-min_rbc57q.png)

World Domination Nation, a woman-led consulting powerhouse, propels individuals and organizations to unleash their potential through visionary "world domination plans." With strategic prowess, innovative spirit, and meticulous execution, we've revolutionized urban sustainability, pioneered health-tech breakthroughs, reignited space exploration, transformed education, harnessed AI for social good, and ignited culinary fusion. Through our diverse projects, World Domination Nation showcases the remarkable impact of our empowering approach, reshaping industries and inspiring positive global change.

WDN Consulting has a series of key partners who contribute to the success of our business:

| Partner Name       | Relationship                                          |
| ------------------ | ----------------------------------------------------- |
| kayseeseven.com    | KaySeeSeven is a nonprofit organization dedicated to empowering individuals from diverse backgrounds by providing comprehensive support and guidance to help them establish successful careers in the tech industry. Through mentorship, training, and networking opportunities, KaySeeSeven aims to bridge the diversity gap in the tech sector. |
| daiwokfoods.com    | Daiwok Foods is a premium fine food eatery renowned for its exquisite Asian culinary offerings. With a commitment to culinary excellence, Daiwok Foods takes diners on a delightful gastronomic journey through the rich and diverse flavors of Asian cuisine. From traditional favorites to innovative creations, they offer a memorable dining experience that celebrates the art of food. |
| castleandsand.com  | Castle & Sand is a reputable company that specializes in providing a wide range of high-quality beach supplies and equipment. From beach umbrellas and chairs to recreational gear, they offer everything needed for a perfect day at the beach. With a focus on durability and functionality, Castle & Sand ensures that beachgoers have the best products to enhance their seaside adventures. |
| getyabreadup.com   | GetYaBreadUp is a distinguished financial advisory firm that prioritizes comprehensive wealth management strategies for its clients. With a team of seasoned experts, they offer personalized financial planning, investment guidance, and estate management services to help clients achieve their financial goals and secure their financial future. |


WDN Consulting has been laser-focused on broadening our customer base. Recently, though, we have been getting a lot of attention. All this extra attention has brought along some unwelcome visitors- cyber attackers! That’s why we’ve hired you! To help keep us safe!

Like all good companies, WDN Consulting collects log data about the activity our employees perform on the corporate network. These security audit logs are stored in Azure Data Explorer (ADX) - a data storage service in Azure (Microsoft’s cloud). You will use the Kusto Query Language (KQL) to parse through various types of security logs. By analysing these logs, you can help us determine whether we’re being targeted by malicious actors.

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


You can find all the links you need here: [scoreboard.kc7cyber.com/modules/SQUAD](http:scoreboard.kc7cyber.com/modules/SQUAD)

### Legend

> 🎯Key Point – Occasionally, you will see a dart emoji with a “key point.” These signal explanations of certain concepts that may enhance your understanding of key cybersecurity ideas that are demonstrated in the game. 

> 🤔Question – “Thinking” emojis represent questions that will enable you to demonstrate mastery of the concepts at hand. You can earn points by entering your responses to questions from section 3 in the scoring portal available at kc7cyber.com/scoreboard.

> 🤫 Hint – “Whisper” emojis represent in-game hints. These hints will guide you in the right direction in answering some of the questions.

## Section 1: The Walkthrough 

#### Getting Set Up in Azure Data Explorer (ADX)

ADX is the primary tool used in the WDN Consulting SOC for data exploration and analysis. The great thing about ADX is that it is used by cyber analysts at many of the smallest and largest organizations in the world. 

Let’s get you logged in and started with ADX:

1. Go to the WDN Consulting training module at [scoreboard.kc7cyber.com/squadcon](scoreboard.kc7cyber.com/squadcon)

On the training module page, you’ll see a button that says **Get the Data**. Click this and it will redirect you to ADX! (Note: You’ll probably be asked to login with a Microsoft account. You can use an existing personal or organization-issued Microsoft account, or create a new one for free.)

Once you login, you should see a cluster called  _“kc7001.eastus”_   has already been added to your account.

<img width="320" alt="image" src="https://github.com/KC7-Foundation/kc7_data/assets/9474932/b86cad4f-66fc-4cc8-bab6-2ef3917b8e01">


Data in ADX is organized in a hierarchical structure which consists of **clusters**, **databases**, and **tables**.

<img width="502" alt="image" src="https://github.com/KC7-Foundation/kc7_data/assets/9474932/706f2c64-7728-4902-ad5f-0d4726f539f9">


All of WDN Consulting’s security logs are stored in a single database – the DominationNation database. 

  2. Select your database.  
	- Expand the dropdown arrow next to the WDN Consulting database.
	- Click on the **DominationNation** database. Once you’ve done this, you should see the database highlighted- this means you’ve selected the database and are ready to query the tables inside.

Note: It’s very important that you use the DominationNation database for all questions while you’re investigating activity at WDN Consulting! If you choose the wrong database, you won’t be able to answer questions correctly.

The big space to the right of your cluster list is the _query workspace_. That’s where you’ll actually write the queries used to interact with our log data.

<img width="839" alt="image" src="https://github.com/KC7-Foundation/kc7_data/assets/9474932/e654b2e7-2436-4012-823c-0803f617a79a">


Currently, you’ll see there’s a message there welcoming you to WDN Consulting! Click the blue Run button above the query workspace to run your first query! Once you’ve done that, you can erase the welcome message by highlighting it and pressing backspace or delete on your keyboard.

Okay, enough introductions… let’s get your hands on the data.

#### First Look at the data... 

The **DominationNation** database contains nine tables. Tables contain many rows of similar data. For security logs, a single row typically represents a single thing done by an employee or a device on the network at a particular time.

We currently have nine types of log data. As you’ll see in ADX, each log type corresponds to a table that exists in the **DominationNation** database:

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


The Employees table contains information about all the employees in our organization. In this case, we can see that the organization is named “WDN Consulting” and the domain is “wdnconsulting.com”.

>1. 🤔 Try it for yourself! Do a **take** 10 on all the other tables to see what kind of data they contain.

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

2.  🤔How many employees are in the company?

**Filtering Data With the _where_ Operator**

So far, we’ve run queries that look at the entire contents of the table. Often in cybersecurity analysis, we only want to look at data that meets a set of conditions or criteria. To accomplish this, we apply filters to specific columns.
  
We can use the **where** operator in KQL to apply filters to a particular field. For example, we can find all the employees with the name “Steven” by filtering on the name column in the Employees table.

**where** statements are written using a particular structure. Use this helpful chart below to understand how to structure a **where** statement.

| **where** | **field** | **operator** | **"value"** |
| ----------- | ----------- | ----------- | ----------- |
| where | name | has | "Steven" |

```sql
Employees
| where name has "Steven"
```

The **has** operator is useful here because we’re looking for only a partial match. If we wanted to look for an employee with a specific first and last name (an exact match), we’d use the == operator:

```sql
Employees
| where name == "Steven Walker"
```

>3. 🤔Each employee at WDN Consulting is assigned an IP address. Which employee has the IP address: “192.168.2.191”?

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


While performing their day-to-day tasks, WDN Consulting employees send and receive emails. A record of each of these emails is stored in the **Email** table.

> 🎯**Key Point – User Privacy and Metadata**: As you can imagine, some emails are highly sensitive. Instead of storing the entire contents of every email sent and received within the company in a database that can be easily accessed by security analysts, we only capture email metadata. 

Email metadata includes information like: the time the email was sent, the sender, the recipient, the subject line, and any links the email may contain. Storing only email metadata, rather than entire contents, helps protect the privacy of our employees, while also ensuring that our security analysts can keep us safe. Sometimes even metadata can reveal sensitive information, so it’s important that you don’t talk about log data with other employees outside the SOC.


We can find information about the emails sent or received by a user by looking for their email address in the sender and recipient fields of the **Email** table. For example, we can use the following query to see all the emails sent by “Steven Walker”:

```sql

Email
| where sender == "steven_walker@wdnconsulting.com"

```

> 4. 🤔How many emails did Gloria Gould receive?

**Easy as 1, 2, 3… Compound Queries and the distinct Operator**

We can use the **distinct** operator to find unique values in a particular column. We can use the following query to determine how many of the organization’s users sent emails.

```sql
Email
| where sender has "wdnconsulting.com"
| distinct sender
| count
```

This is our first time using a multi-line query with multiple operators, so let’s break it down:

In line 2, we take the Email table and filter the data down to find only those rows with “wdnconsulting.com” in the sender column.

In line 3, we add another pipe character ( | ) and use the distinct operator to find all the unique senders. Here, we aren’t finding the unique senders for all of the email senders, but only the unique senders that are left after we apply the filter looking for rows with “wdnconsulting.com” in the sender column.

Finally, in line 4, we add another pipe character ( | ) and then use the count operator to count the results of lines 1-3 of the query.

5.  🤔How many distinct senders were seen in the email logs from kayseeseven.com?

**Tracking Down a Click: OutboundNetworkEvents Data**

When employees at WDN Consulting browse to a website from within the corporate network, that browsing activity is logged. This is stored in the **OutboundNetworkEvents** table, which contains records of the websites browsed by each user in the company. Whenever someone visits a website, a record of it is stored in the table. However, the user’s name is not stored in the table, only their IP address is recorded. There is a 1:1 relationship between users and their assigned IP addresses, so we can reference the **Employees** table to figure out who browsed a particular website. When a user visits a site, sometimes data from a lot of other sources are loaded as well. For example, images, assets, and other content may be hosted on content delivery network (CDN), which is used to deliver and load content quickly on a website. Sometimes, advertisements will also load from a particular website as well.

If we want to figure out what websites Judith Sower visited, we can find their IP address from the Employees table.

```sql
Employees
| where name == "Judith Sower"
```

The query above tells us their IP address is “10.10.0.95”. We can take their IP address and look in the **OutboundNetworkEvents** table to determine what websites they visited.

```sql
OutboundNetworkEvents
| where src_ip == "10.10.0.95"
```

> 6.  🤔How many unique websites did “Judith Sower” visit?


**What’s in a Name? All about Passive DNS Data**

Although domain names like “google.com” are easy for humans to remember, computers don’t know how to handle them. So, they convert them to machine readable IP addresses. Just like your home address tells your friends how to find your house or apartment, an IP address tells your computer where to find a page or service hosted on the internet.

> 🎯**Key Point – Practice Good OPSEC**: If we want to find out which IP address a particular domain resolves to, we could just browse to it. But, if the domain is a malicious one, you could download malicious files to your corporate analysis system or tip off the attackers that you know about their infrastructure. As cybersecurity analysts, we must follow procedures and safeguards that protect our ability to track threats. These practices are generally called operational security, or OPSEC.

To eliminate the need to actively resolve (that is- directly browse to or interact with a domain to find it’s related IP address) every domain we’re interested in, we can rely on passive DNS data. Passive DNS data allows us to safely explore domain-to-IP relationships, so we can answer questions like:

-   *Which IP address does this domain resolve to?*
-   *Which domains are hosted on this IP address?*
-   *How many other IPs have this domain resolved to?*

These domain-to-IP relationships are stored in our **PassiveDns** table.

> 7. 🤔 How many domains in the PassiveDns records contain the word “crypto”?
> 8. 🤔 What IPs did the domain “cryptocoin.net” resolve to (enter any one of them)??

**🤯Let statements – making your life a bit easier:**

Sometimes we need to use the output of one query as the input for a second query.  The first way we can do this is by manually typing the results into the next query.

For example, what if we want to look at all the web browsing activity from employees named “James”?

First, you would need to go into the **Employees** table and find the IP addresses used by these employees.


```sql
Employees
| where name has "James"
```

![](https://lh5.googleusercontent.com/Ul2NZuxCWG8Wg3zIGBjTSrUQHeNJ7b_cPKWSqvqGrkptYJ8F0Y4-qtoTcR8OtjJGzQaCqdyDe9_fJDSXOm58hWdgvNYqvteckmtEvq51WIjYrxXIIi-rdGUSIZ0eSYon7mOoL2jFrmu-)

Then, you could manually copy and paste these IPs into a query against the **OutboundNetworkEvents** table. Note that we can use the in operator to choose all rows that have a value matching any value from a list of possible values. In other words, the == (comparison) operator looks for an exact match, while the in operator checks for any values from the list

```sql 
OutboundNetworkEvents
| where src-ip in ("192.168.1.132",
	"192.168.3.128",
	"192.168.1.70",
	"192.168.1.92",
	"192.168.0.147",
	"192.168.1.50",
	"192.168.0.163",
	"192.168.2.200")
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

> 9.  🤔 How many unique URLs were browsed by employees named “Karen”?

> 🎯**Key Point – Pivoting:** Part of being a great cyber analyst is learning how to use multiple data sources to tell a more complete story of what an attacker has done. We call this “pivoting.” We pivot by taking one known piece of data in one dataset and looking in a different dataset to learn something we didn’t already know. You practiced this here when we started in one dataset – the Employees table – and used knowledge from there to find related data in another source – OutboundNetworkEvents.


## Section 2: Start Hunting! 

You’ve finished your training and you’re ready to get to work protecting WDN Consulting. 

Work with your team to complete as many challenge questions from the remaining sections in the scoreboard as possible! The goal is to score as many points as you can. There are a lot of questions (the attackers have been busy), so you probably won’t be able to answer them all. Just do as many as you can!

As you answer the questions, we will take you on a journey exploring the data and discovering what actions the adversaries have taken. However, you should remember that this is only one of many paths you can take through the data.  As you go, don’t forget to pay attention to the details along the way. What patterns do the attackers exhibit that could help you track them better? Do they like to use certain words, themes? Or do they make mistakes? Keeping track of these patterns will help you build the full picture of what happened. 

Use the provided [Actor Preview](https://docs.google.com/document/d/1rZR4eVG886oPziG-5nGeQ5kN_q5Bpq0m/edit?usp=sharing&ouid=105873493764084037775&rtpof=true&sd=true) document to keep track of what you know about the attacker. Building a good profile, timelining the attacker’s activity, and forming a list of indicators of compromise (IOCs) will help you keep track of the attacker. KC7 models some of the techniques used by these attackers from real-world threat actors, so it may be a helpful resource for you in the future when you are investigating a real security incident. 

Now, get out there and keep us safe! The whole company is counting on you. No pressure😊.


## Resources

Understanding KQL operators: [https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators)
