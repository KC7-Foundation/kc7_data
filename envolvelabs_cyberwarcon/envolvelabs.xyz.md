# Welcome to EnvolveLabs!

🥳 Today is your first day as a Junior Security Operations Center (SOC) analyst. Your job is to safeguard EnvolveLabs, a leading research hub in Ukraine, and its dedicated team from potential cyber threats, especially during these challenging times of the Russian invasion.

![2](https://github.com/KC7-Foundation/kc7_data/assets/9474932/db6fffb7-6f81-4fc1-ada7-ab0af4700732)


EnvolveLabs, situated in Ukraine's vibrant tech landscape, is a nexus of groundbreaking research in the intertwined fields of energy and artificial intelligence (AI). This research institute is harnessing renewable energy sources, all while employing advanced AI algorithms to optimize energy production, distribution, and consumption. Beyond just energy, they're leveraging AI to predict energy demands, streamline grid operations, and even forecast environmental impacts.

However, in the wake of the Russian invasion, their cutting-edge research and strategic location have inadvertently made EnvolveLabs a prime target for cyber threats and espionage. Your mission, if you choose to accept it, is to investigate attacks against EvolveLabs so they can help keep the lights on for everyone in the country.

EnvolveLabs has some key partners that contribute to the success of its research endeavors.

| Partner Name | Relationship |
|--------------|--------------|
| We Sell Beakers (wesellbeakers.online) | We Sell Beakers is a crucial partner for EnvolveLabs, providing essential laboratory equipment and supplies to support their groundbreaking research. |
| Ukrainian Pharma Supplies (pharmasupplies.shop) | Ukrainian Pharma Supplies plays a significant role in supporting EnvolveLabs' research activities, particularly in the context of healthcare and pharmaceutical-related projects. |
| The Center For Compliance in Research Ethics (researchcompliance.biz) | The Center for Compliance in Research Ethics is a valued partner, providing guidance and expertise in ensuring that EnvolveLabs' research adheres to the highest ethical and compliance standards. |
| Vaccine Distributors Inc (vaxdistro.site) | Vaccine Distributors Inc plays a crucial role in public health initiatives and is a key collaborator of EnvolveLabs. Protecting this partnership is vital for maintaining public health and safety. |

## Objectives

🧠 **By the end of your first day on the job, you should be able to:**
	
- Pivot on data using Storm and Synapse, learning how to perform Lift, Filter and Pivot operations. 
- Learn how to label nodes to form an analytical layer. 
- Use multiple datasets to answer targeted questions.
- Investigate cyber activity in logs including: email, web traffic, and server logs.
- Use multiple techniques to track the activity of APTs (Advanced Persistent Threats).
- Use third-party datasets to discover things about your attackers.
- Make recommendations on what actions a company can take to protect themselves.



🚀 The attackers have gotten a head start, so let's not waste any more time... **let's get to work!**


## Getting Set Up in Synapse

A few things you’ll need to do first:

1. Request a Synapse Enterprise demo instance at: [https://vertex.link/request-a-demo](https://vertex.link/request-a-demo).

2. Once you’ve requested a Synapse demo instance, you will receive an email from `Optic <signup@vertex.link>` with a link to your Synapse demo instance. Click the link to create a password and log into your Synapse instance.

3. Once you’ve logged in, you should be in the **Research Tool**, which you can verify by checking the green Top Bar, at the top of the UI. You should see  Research in the left hand corner. You can also navigate to the **Research Tool** by clicking on the magnifying glass icon at the top of the Tool Bar, on the left side of the page.

   
 	![Click on Research](https://github.com/KC7-Foundation/kc7_data/assets/9474932/3c2087c4-447f-4ba8-a0cf-9b4db0a33d14)

4. Next, use the **Workspace Selector** dropdown menu in the Top Bar to change from “default” to the _KC7 Workspace_. This will be the first dropdown menu to the right of where it says Research.
 

5. After selecting the KC7 Workspace, move to the dropdown menu to the right of the **Workspace Selector**. This is called the **View Selector**. Next, select the _KC7-EnvolveLabs_ View. This is where the data for today’s exercise lives. The Top Bar of your UI should now look like this:
   
	![top bar of UI with KC0-Envolvelabs view](https://github.com/KC7-Foundation/kc7_data/assets/9474932/76302c4f-e526-4b46-ba40-62215f2d75f5)

6. Next, we’re going to create a scratchspace to work out of. To do this, click the **Fork** icon to the right of the **View Selector**:
   
	![image](https://github.com/KC7-Foundation/kc7_data/assets/9474932/1ace5c60-a5ad-4df0-b6e1-dc673ae8a569)


	This will give you a pop-up window where you can name your “scratchspace”, then click **Fork** to create it. You’ll see the name of your scratchspace replace the _KC7-EnvolveLabs_ View in the Top Bar.


7. Double check that the **Query Bar**, shown below, is set to Storm mode:
   
	![active storm mode](https://github.com/KC7-Foundation/kc7_data/assets/9474932/9df6e908-2f5c-444c-afdf-7c2d6826d068)


	We’ll be using this **Query Bar** to run our queries for today’s exercises. 


8. Use the Tool Bar on the left of the UI, navigate to the **Power-Ups Tool** by clicking on the lightning bolt icon. Once in the **Power-Ups Tool**, use the search bar to search for the _Synapse-Alienvault_ Power-Up under the _Available_ tab. Click the **Add** button to install it on your Synapse Instance:
	
 	![power-up_install](https://github.com/KC7-Foundation/kc7_data/assets/9474932/4121af76-4f42-4ae2-811a-76b427eefc23)


9. Once you’ve installed the Power-Ups, use the Tool Bar on the left to navigate to the **Console Tool** by clicking on the icon, shown below: 

10. Type the following command into the Query Bar and hit Enter to set your Alienvault API key:

```js
alienvault.setup.apikey <apikey>
```

Once you’ve done that, use the magnifying glass icon at the top of the left-hand toolbar to return to the Research Tool. 

## Section 1: The Walkthrough

### First Look at the Data…

EnvolveLabs maintains a log of various events taking place within the company. This data is gathered from multiple sources such as network sensors, firewalls, and endpoint products. This information is pivotal for detecting and countering malicious activities. You can access this data in Synapse using the following forms:

| Form Used           | Description                                                                                          |
|---------------------|------------------------------------------------------------------------------------------------------|
| `ps:contact`        | Information about employees hired at the company and their corresponding assets                      |
| `inet:email:message`| Emails sent and received by employees at the company                                                 |
| `it:host`           | Information about hosts assigned to the company’s employees                                          |                                                                                     |
| `inet:http:request` | Web browsing activity to and from the Internet, indicating websites visited by our employees and external users browsing our websites |
| `it:logon`          | Successful and failed logins to devices on the company network, including logins to the company’s mail server |
| `it:exec:file:add`  | Data about one of the employee-assigned host machines adding a file to a filesystem                  |
| `it:exec:proc`      | Data regarding processes created on employee’s devices                                               |
| `inet:dns:a`        | IP-domain resolutions provided by a third-party vendor                                               |
| `risk:alert`        | Security alerts from employees’ devices and the company’s email security system                      |

<br>

These additional forms may be helpful during your investigations

| Form                      | Description                                      |
|---------------------------|--------------------------------------------------|
| `inet:email`              | An email address                                 |
| `inet:email:message:link` | A URL/link embedded in an email message          |
| `inet:fqdn`               | A fully qualified domain name                    |
| `inet:ipv4`               | An IPv4 address                                  |
| `inet:url`                | A URL                                            |
| `file:base`               | A filename                                       |
| `ou:jobtitle`             | A job title                                      |


<br>

<mark style="background-color: #FFFF00">
	
**A Note on Forms, Properties, and Nodes:**

It can be helpful to think of a [Form](https://synapse.docs.vertex.link/en/latest/synapse/userguides/data_model.html#form) as a template for capturing a certain kind of information in Synapse. For example, EnvolveLabs uses the ps:contact form to capture individual contact information about the company’s employees. Forms have [properties](https://synapse.docs.vertex.link/en/latest/synapse/userguides/data_model.html#property), which are specific fields that provide additional information. Properties found on the `ps:contact` form, for example, include `:name`, `:email`, `:title`, and `:type`, among others. 

We can use a form to capture information within Synapse - filling out the template as it were - at which point we refer to that information as a node. The image below shows the `ps:contact` node for EnvolveLabs employee Cecil Bryant:

![cecil bryant employee](https://github.com/KC7-Foundation/kc7_data/assets/9474932/93b611dc-73e7-4906-b848-d635d0083eb3)

The properties displayed are `:name`, `:title`, `:user`, `:email`, and `:type`, the last of which categorizes this entry as an employee. 

You can review all currently available Forms within Synapse in the **Data Model Explorer**. The **Data Model Explorer** is an index of all Forms in Synapse and provides information such as, what the Form is used to capture, which properties are available, and how the Form relates to others within Synapse’s data model. You can access the **Data Model Explorer** via the **Help Tool**, as shown below:

![data_model_explorer](https://github.com/KC7-Foundation/kc7_data/assets/9474932/a6456f9e-db81-45d6-846c-575de195d9a1)
</mark>

## STORM 101

There are three basic operations we’ll use to query the company’s logs using Storm: **Lift, Filter, and Pivot**. We can build everything on top of these basics. 

In some cases, we can use the Synapse UI to answer the question rather than running a Storm query. We’ll point out how to do that when possible.



### Lift

[Lift operations](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_lift.html) retrieve a set of nodes based on specified criteria. Let’s try a few lifts to get comfortable using Storm and working with the EnvolveLabs datasets.

<br>

<u>**Lifts - Example 1: Let’s find all the employees at EnvolveLabs**</u>

To look at all the employees in the EnvolveLabs data, we can use a lift operation. We need to start by typing the Form for the employee object. Here, we are capturing information about employees using the form `ps:contact`.

Let’s type that into the Query Bar and press Enter. We’ll find there are 1,513 results. Each result is a node that represents a unique employee at the company, with more detail about each employee - such as name, email address, and title - captured as a node property. 

> Question 1: How many employees are in the company?

Note that we can use this query to lift only `ps:contact` nodes for EnvolveLabs employees since those are currently the only `ps:contact` nodes in this data set. We would need to run a more specific query (such as `ps:contact:type=employee`)  if this data set also included contact information for other individuals. 

<br>  

<u>**Lifts Example 2: Finding a limited number of nodes**</u>

In <u>Lifts - Example 1</u>, we found all the employees at EnvolveLabs by typing the name of the Form `ps:contact` that corresponds to the employee data model and lifting all `ps:contact` nodes. But, what if we wanted to look at an even larger set of nodes, for instance, all the domains seen in EnvolveLabs data?

As with <u>Example 1</u>, let’s start by typing the Form name associated with domains:

```css
inet:fqdn
```

This query brings back over 2,000 results. To see a more manageable set of the data, we can pipe the output of the query to the limit command, which will only return the specified numbers of results. Let’s run this query to see only 10 domains:

```css
inet:fqdn | limit 10
```

>Question 2: Lift 10 employees. Copy and paste the query you used. 

A note on Storm commands:

There are several [Storm commands](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_cmd.html) that you may find useful when querying the data. These include:

| Command | Description                                                         |
|---------|---------------------------------------------------------------------|
| count   | Iterate through query results, and print the resulting number of nodes to the Console Tool |
| limit   | Limit the number of nodes generated by the query to a specific number of results |
| min     | Lift the resulting node with the lowest value for the specified property |
| max     | Lift the resulting node with the greatest value for the specified property |
| uniq    | Remove duplicate nodes from the query results                       |


When incorporating a Storm command into a query, we’ll need to use the pipe (“|”) character to pipe the inbound nodes to the command. A query to lift the most recently received email message modeled in our Synapse instance will  look like this:

```css
inet:email:message | max :date
```

 If we want to continue our query after the Storm command, we’ll need to use the pipe character once more between the Storm command and the rest of the query. As an example, if we take the query above to lift the most recently received email message, and pivot to any embedded URLs sent within the email, our query will look like this:

```css
inet:email:message | max :date | -> inet:email:message:link
```

<br>
 
<u>**Lifts Example 3: Standard and Extended Comparisons**</u>

Lift can do more than just show us all nodes in the hypergraph. Often, we want to use a Storm query to find only specific nodes that meet a given set of criteria. Suppose we wanted to find more information about EnvolveLabs employee Stephanie Clark.

We’ll be looking at `ps:contact` nodes for this query, but this time, we’ll focus specifically on the node’s `:name` property. Using the `=` comparator, we can find the exact node that belongs to Stephanie Clark:

```css
ps:contact:name="Stephanie Clark"
```

If, instead, we wanted to find all EnvolveLabs employees with the first name Stephanie, we could use the prefix comparator `^=` to match these nodes:

```css
ps:contact:name^=Stephanie
```

There are several other comparison operators that we may find useful going forward. These include:

| Operator | Lift by                                   |
|----------|-------------------------------------------|
| =        | Exact value                               |
| ^=       | Prefix                                    |
| ~=       | Regular expression                        |
| @=       | Time or Interval                          |
| >        | Value is greater than                     |
| <        | Value is less than                        |
| >=       | Value greater than or equal to            |
| <=       | Value less than or equal to               |

Here are some example Lift operations using [Standard Common Operators](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_lift.html#lifts-using-standard-comparison-operators): 

| Description                                                       | Command                                            |
|-------------------------------------------------------------------|----------------------------------------------------|
| Lift email messages sent from the email address, “`gregorysmith@aol.com`” | `inet:email:message:from=gregorysmith@aol.com`     |
| Lift email messages whose subject starts with the word, “`Important`” | `inet:email:message:subject^=Important`            |
| Lift alerts containing the process “`waitfor.exe`” in the description | `risk:alert:desc~=waitfor.exe`                     |
| Lift HTTP requests that took place between January 1, 2023 (“`20230101`”) and January 31, 2023. | `inet:http:request:time@=(20230101,20230201)`      |
|                                                                   | `inet:http:request:time@=(20230101, “+31 days”)`   |
| Lift email messages sent after November 8, 2023 (“`20231108`”)      | `inet:email:message:date>20231108`                |
| Lift email messages sent before November 8, 2023 (“`20231108`”)     | `inet:email:message:date<20231109`                |
| Lift process execution events that occurred on or after November 15, 2023 (“`20231115`”) | `it:exec:proc:time>=20231115`                    |
| Lift process execution events that occurred on or before November 15, 2023 (“`20231115`”) | `it:exec:proc:time<=20231115`                    |


>Question: 3 What is the CEO’s name? (hint: lift by the title)


<br>

### Filter

[Filter operations](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_filter.html#simple-filters) are performed on the output of a previous Storm operation, such as a lift or pivot. We use filter to downselect the set of nodes by either including or excluding nodes based on a set of criteria:

* `+` specifies an _inclusion filter_ (include **only** the nodes that match the criteria)
* `-` specifies an _exclusion filter_ (include all nodes **except** those that match the criteria)

<br>

<u>**Filters Example 1: Inclusion Filtering**</u>

Suppose we wanted to look at all the emails sent by Stephanie Clark, but only those emails about economics.

First, let’s find Stephanie’s email, which we can do using a simple lift:

```css
ps:contact:name="Stephanie Clark"
```

Right-click on Stephanie’s email address from the results table to copy it. Next, we need to identify the form which corresponds to email messages sent to/from the company.

```css
inet:email:message
```

Then, we can use a simple lift to find emails sent by Stephanie:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz"
```

Now, we’ve found all the emails sent by Stephanie. But, remember, we need to find only those messages about economics. We can add a filter based on the subject line of the email messages to find such emails. In this case, we’ll use an inclusion filter with a [Regular Expression (regex) comparator](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_lift.html#lift-by-regular-expression) to look for the term “economics” anywhere within the email’s Subject line:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" +:subject~="economics"
```

This leaves us with only one result.

>Question 4: Who was the first person to send Delia Hill an email about “solidarity”? 

  
<u>**Filters Example 2: Exclusion Filtering**</u>

Let’s look again at Stephanie’s emails. But this time, we want to ignore any email that might mention an opinion. For this, we’ll use an exclusion filter:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" -:subject~="opinions"
```

This leaves us 17 results.

>Question 5: How many emails did Yuko Sidney send that don’t mention the word “term”?

<br>
  
### Pivot

[Pivot](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_pivot.html#) and [Traverse](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_pivot.html#traverse-walk-light-edges) operations are performed on the output of a previous Storm operation such as a Lift or Filter. These operations are used to navigate from one set of nodes to another based on a specified relationship. 

Pivoting involves lifting adjacent nodes that share property values. However, traversing involves walking a named lightweight (light) edge (e.g., _refs_) to lift connected nodes.

We mentioned earlier that forms have properties, which are specific fields that hold information about the node modeled in Synapse. Each form has, at a minimum, a primary property, which is the name of the form and definition of the value for individual instances of that form, and which a user must populate in order to create the node within Synapse. For some forms, the primary property value may be a Globally Unique Identifier (GUID). For the `inet:email` node shown below, the primary property value for is "alexander_hall@envolvelabs.xyz":

```css
inet:email=alexander_hall@envolvelabs.xyz
```

Many forms also have secondary properties, which are optional fields capable of holding additional information, but which are not necessary for creating the node within Synapse. The `inet:email:message form`, for example, has multiple secondary properties, such as `:date`, `:body`, `:bytes`, `:to`, and `:from`, among others. We can pivot from the `inet:email=alexander_hall@envolvelabs.xyz` to all associated `inet:email:message` nodes that share the value "alexander_hall@envolvelabs.xyz". While "alexander_hall@envolvelabs.xyz" is the primary property value for the `inet:email` node, it may appear as one of several secondary property values (`:to`, `:from`, `:replyto`) for the associated `inet:email:message` nodes.

![image6](https://github.com/KC7-Foundation/kc7_data/assets/9474932/058bce1f-d96d-484a-b12c-e78bbc47fa43)

Pivoting from `inet:email=alexander_hall@envolvelabs.xyz` to associated `inet:email:message` nodes using Storm would look like the following (note the use of “->”, which is the [Pivot Out Operator](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_pivot.html#pivot-out-operator):

```css
inet:email=alexander_hall@envolvelabs.xyz -> inet:email:message
```

However, it is also possible to lift `inet:email=alexander_hall@envolvelabs.xyz` and pivot out to all associated `inet:email:message` nodes without using Storm at all. We can do this by switching the Query Bar from Storm to Lookup mode, querying alexander_hall@envolvelabs.xyz, and then using the Explore button to pivot to connected nodes, including `inet:email:message nodes`: 

![explore_ui (1)](https://github.com/KC7-Foundation/kc7_data/assets/9474932/c0775745-e5a0-41fc-98f6-c006ed09eb0d)

While it’s possible to use the UI, or a combination of the Storm query language and the UI, to query the data in Synapse, doing so is generally less efficient than running a Storm query. However, when it comes to pivoting, using the **Explore** button to view adjacent nodes can be a way for analysts to get a sense of how nodes are connected within Synapse and what pivots they will need to make in their Storm query. 

<br>
  
<u>**Pivot Example**</u>

Suppose you want to find all of the links sent in the emails sent by Stephanie Clark. We can accomplish this by pivoting. 

First, lift the emails sent by Stephanie. Run the following Storm query to lift them by the `:from` property and value:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" 
```

Then pivot from those messages to those that also include embedded links:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" -> inet:email:message:link 
```

Then, we pivot from the `inet:email:message:link` nodes representing those messages to the `inet:url` nodes representing just the URLs themselves.

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" -> inet:email:message:link -> inet:url
```

Of course, there could have been multiple links that were sent by Stephanie. We can view the unique links by piping the existing query to the `uniq` command. 

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" -> inet:email:message:link -> inet:url | uniq
```

>Question 6: How many emails did the CEO receive?

>Question 7: How many unique links were sent to the CEO via email?

>Question 8: How many unique websites did the CEO visit?

>Question 9: How many domains in the PassiveDNS records contain the word “cloudapps”?

>Question 10: What IPs did the domain security-cloudapps.envolvelabs.xyz resolve to?

  
<u>**Traverse Example**</u>

In a few specific instances, nodes are connected by lightweight (light edges) representing a specific, directional relationship between the nodes. For example, a blog modeled as a media:news node that references multiple indicators will have a “refs” lightweight edge pointing to the nodes representing those referenced indicators. In this data set, lightweight edges appear between risk:alert nodes and nodes that the alerts reference. The query below shows a pivot from a risk:alert node to the file:bytes node that the alert references:

```css
risk:alert:detected="2023/11/13 15:51:25" -(refs)> file:bytes
```
Note here that if you only relied on pivoting, you would have no way of getting to the file:bytes from risk alerts. Run the following queries and examine the difference.

```
risk:alert:detected="2023/11/13 15:51:25" ->*

risk:alert:detected="2023/11/13 15:51:25" -(refs)>*
```

>Question 11: What is the job role of the employee assigned to the host machine from the risk alert that was detected on 2023/10/27 15:26:47? 

> Question 12: Who sent the email that mentions “german” that was detected in the alerts?


## Section 2 Start Hunting!

You’ve finished your training and you’re ready to get to work protecting EnvolveLabs.

Work with your team to complete as many challenge questions from the remaining sections in the scoreboard as possible! The goal is to score as many points as you can. There are a lot of questions (the attackers have been busy), so you probably won’t be able to answer them all. Just do as many as you can!

As you answer the questions, we will take you on a journey exploring the data and discovering what actions the adversaries have taken. However, you should remember that this is only one of many paths you can take through the data. As you go, don’t forget to pay attention to the details along the way. What patterns do the attackers exhibit that could help you track them better? Do they like to use certain words, or themes? Or do they make mistakes? Keeping track of these patterns will help you build the full picture of what happened.

Use the provided Actor Preview document to keep track of what you know about the attacker. Building a good profile, timelining the attacker’s activity, and forming a list of indicators of compromise (IOCs) will help you keep track of the attacker. KC7 models some of the techniques used by these attackers from real world threat actors, so it may be a helpful resource for you in the future when you are investigating a real security incident.

Now, get out there and keep us safe! The whole company is counting on you. No pressure😊.

![cat typing gif](https://media.tenor.com/CrWHpzxIZYEAAAAC/cat-typing-gif.gif)

## Section 3: Clustering!

Now that we learned more about the Techniques, Tactics and the Procedures, used by the adversaries, we want to start documenting what we know so we can answer meaningful questions and build detections to block them. Instead of storing what we know about the adversaries as notes on a piece of paper, or just written words that someone else will need to re-interpret, we will label the relevant nodes using [tags](https://synapse.docs.vertex.link/en/latest/synapse/userguides/analytical_model.html#) to keep track of our findings. 

Synapse gives you a structured way to record observations or assessments through the use of labels (tags) applied to data (nodes). Assessments represent conclusions based on the data available to you at the time. Tags are flexible and can be easily added, updated, or removed when assessments change, meaning that you can revise your analysis as needed when  new data becomes available.

For simplicity, we’ll nickname the first threat actor we are tracking “lightning.” We’ll use the tag  `#cno.threat.lightning` to label all nodes that correspond to this threat actor. You can apply tags to a node by right-clicking on the node in the **Results** pane of the **Research Tool**, and then selecting _Add Tags_ from the context menu, as shown below:

![tag_add1 (1)](https://github.com/KC7-Foundation/kc7_data/assets/9474932/e2efea45-4a75-452b-82dd-8d1f6b5f116d)

Alternatively you can select the node and click the _Add Tags_ button on the **Details** pane on the right-hand side:

![tag_add2 (1)](https://github.com/KC7-Foundation/kc7_data/assets/9474932/f840e27a-094c-456a-8a47-7fefa80cb9b6)

Once you’ve applied the tag, you’ll see it appear in the **Details** pane view for the tagged node. You can remove the tag by clicking on it in the **Details** pane and selecting _Remove full tag_:

![tag_remove (1)](https://github.com/KC7-Foundation/kc7_data/assets/9474932/b8c65166-3ab7-45cb-a4b2-764dbc2d2b17)

## References

- Storm Reference: [https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_intro.html](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_intro.html)
- Maxmind Power-Up: [https://synapse.docs.vertex.link/projects/maxmind/en/latest/](https://synapse.docs.vertex.link/projects/maxmind/en/latest/)
- Alientvault OTX Power-Up: [https://synapse.docs.vertex.link/projects/rapid-powerups/en/latest/storm-packages/synapse-alienvault/userguide.html](https://synapse.docs.vertex.link/projects/rapid-powerups/en/latest/storm-packages/synapse-alienvault/userguide.html)


