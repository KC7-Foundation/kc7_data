# Welcome to EnvolveLabs!

🥳 **Today is your inaugural day as a Junior Security Operations Center (SOC) analyst.** Your job is to safeguard EnvolveLabs, a leading research hub in Ukraine, and its dedicated team from potential cyber threats, especially during these challenging times of the Russian invasion.

![2](https://github.com/KC7-Foundation/kc7_data/assets/9474932/db6fffb7-6f81-4fc1-ada7-ab0af4700732)


EnvolveLabs, situated in Ukraine's vibrant tech landscape, is a nexus of groundbreaking research in the intertwined fields of energy and artificial intelligence (AI). This research institute is harnessing renewable energy sources, all while employing advanced AI algorithms to optimize energy production, distribution, and consumption. Beyond just energy, they're leveraging AI to predict energy demands, streamline grid operations, and even forecast environmental impacts.

However, in the wake of the Russian invasion, their cutting-edge research and strategic location have inadvertently made EnvolveLabs a prime target for cyber threats and espionage. Your mission, if you choose to accept it, is to investigate attacks against EvolveLabs so they can help keep the lights on for everyone in the country.

EnvolveLabs has some key partners that contribute to the success of its research endeavors.

| Partner Name | Relationship |
|--------------|--------------|
| We Sell Beakers (wesellbeakers.online | We Sell Beakers is a crucial partner for EnvolveLabs, providing essential laboratory equipment and supplies to support their groundbreaking research. |
| Ukrainian Pharma Supplies (pharmasupplies.shop) | Ukrainian Pharma Supplies plays a significant role in supporting EnvolveLabs' research activities, particularly in the context of healthcare and pharmaceutical-related projects. |
| The Center For Compliance in Research Ethics (researchcompliance.biz) | The Center for Compliance in Research Ethics is a valued partner, providing guidance and expertise in ensuring that EnvolveLabs' research adheres to the highest ethical and compliance standards. |
| Vaccine Distributors Inc (vaxdistro.site) | Vaccine Distributors Inc plays a crucial role in public health initiatives and is a key collaborator of EnvolveLabs. Protecting this partnership is vital for maintaining public health and safety. |

## Objectives

🧠 **By the end of your first day on the job, you should be able to:**
	
- Pivot on data using Storm and Synapse, learning how to lift, filter and pivot. 
- Learn how to label nodes to form an analytical layer. 
- Use multiple data sets to answer targeted questions.
- Investigate cyber activity in logs including: email, web traffic, and server logs.
- Use multiple techniques to track the activity of APTs (Advanced Persistent Threats).
- Use third party data sets to discover things about your attackers.
- Make recommendations on what actions a company can take to protect themselves.


🚀 The attackers have gotten a head start, so let's not waste any more time... **let's get to work!**


## Getting Set Up in Synapse

A few things you’ll need to do first:

1. If you have not already received a Synapse demo instance, you can request one [here: https://vertex.link/request-a-demo](https://vertex.link/request-a-demo).

2. Once you’ve requested a Synapse demo instance, you will receive an email from `Optic <signup@vertex.link>` with a link to your Synapse demo instance. Click the link to create a password and log into your Synapse instance.

3. Once you have logged in, you should be in the Research Tool, which you can verify by checking the green Top Bar at the top of the UI. It should show “Research” in the left hand corner. You can also navigate to the Research Tool by clicking on the magnifying glass icon at the top of the toolbar along the left side of the page.
   
	<img width="788" alt="image" src="https://github.com/KC7-Foundation/kc7_data/assets/9474932/07fa0ec2-9cda-4e0e-8f88-e551bfddc9f5">

 
5. Use the Workspace Selector dropdown menu in the Top Bar to change from “default” to the “KC7 Workspace”. This will be the first dropdown menu to the right of where it says “Research”.

6. After selecting the KC7 Workspace, move to the dropdown menu to the right of the Workspace selector, and select the KC7 Cyberwarcon 2023 view. This is where the data for today’s exercise is located. The Top Bar of your UI should now have a specific appearance (a visual representation or description would typically follow here).
   
	<img width="857" alt="image" src="https://github.com/KC7-Foundation/kc7_data/assets/9474932/cea6f55d-f8da-4fe1-8b36-6e9dd6321c8e">

7. Next, we’re going to create a scratchspace to work out of. To do this, click the fork icon to the right of the View Selector:
   
	<img width="856" alt="image" src="https://github.com/KC7-Foundation/kc7_data/assets/9474932/8077aaa3-1c3c-4112-b852-1df6b0447efe">

	This will give you a pop-up window where you can name your scratchspace, then click “Fork” to create it. You’ll see the name of your scratchspace replace the “KC7 Cyberwarcon 2023” view in the View Selector.

8. Double check that the Query Bar, shown below, is set to Storm mode:
   
	![active storm mode](https://github.com/KC7-Foundation/kc7_data/assets/9474932/1d639bf0-d45c-4f8c-a06c-281456555457)


	We’ll be using this Query Bar to run our queries for today’s exercises. 


9. Use the Toolbar on the left of the UI to navigate to the Power-Ups Tool by clicking on the lightning bolt icon. Once in the Power-Ups Tool, use the search bar to search for the Synapse-Alienvault Power-Up in the “Available” tab. Click the “Add” button to install it on your Synapse Instance:

	![powerup_install](https://github.com/KC7-Foundation/kc7_data/assets/9474932/1bf09f51-035f-4f99-bbda-3f87d636fbbc)

	Do the same for the Synapse-Maxmind Power-Up.


10. Once you’ve installed the Power-Ups, use the Toolbar on the left of the UI to navigate to the Console Tool by clicking on the Console Tool icon, shown below: 

11. Type the following command into the Query Bar and hit Enter to set your Alienvault API key:

```js
alienvault.setup.apikey <apikey>
```

12. Once you’ve done that, use the magnifying glass icon at the top of the left hand toolbar to return to the Research Tool. 

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


**A Note on Forms, Properties, and Nodes:**

It can be helpful to think of a Form as a template for capturing a certain kind of information in Synapse. For example, EnvolveLabs uses the ps:contact form to capture individual contact information about the company’s employees. Forms have properties, which are specific fields that provide additional information. Properties found on the ps:contact form, for example, include :name, :email, :title, and :type, among others. 

We can use a form to capture information within Synapse - filling out the template as it were - at which point we refer to that information as a node. The image below shows the ps:contact node for EnvolveLabs employee Cecil Bryant:

![image](https://github.com/KC7-Foundation/kc7_data/assets/9474932/93b611dc-73e7-4906-b848-d635d0083eb3)

The properties displayed are `:name`, `:title`, `:user`, `:email`, and `:type`, the last of which categorizes this entry as an employee. 

You can review all currently available forms within Synapse in the Data Model Explorer, which is accessible through the Help Tool. Click on the question mark icon, shown below, in the left hand Tool Bar to access the Help Tool. 

![image4](https://github.com/KC7-Foundation/kc7_data/assets/9474932/32a1098c-63c4-447c-8dc5-89de3f5ab611)

The Data Model Explorer, found in the first tab, serves as an index of all forms in Synapse and provides information such as what the form is used to capture, which properties are available, and how the form relates to others within Synapse’s Data Model. 


## STORM 101

There are three basic operations we’ll use to query the company’s logs using Storm: Lift, Filter, and Pivot. We can build everything on top of these basics. 

In some cases, we can use the Synapse UI to answer the question rather than running a Storm query. We’ll point out how to do that when possible.


### Lift

Lift operations retrieve a set of nodes based on specified criteria. Let’s try a few lifts to get comfortable using Storm and working with the EnvolveLabs datasets.

<u>**Lifts - Example 1: Let’s find all the employees at EnvolveLabs**</u>

To look at all the employees in the EnvolveLabs data, we can use a lift operation. We need to start by typing the Form for the employee object. Here, we are capturing information about employees using the form `ps:contact`.

Let’s type that into the Query Bar and press Enter. We’ll find there are 1,513 results. Each result is a node that represents a unique employee at the company, with more detail about each employee - such as name, email address, and title - captured as a node property. 

> Question 1: How many employees are in the company?

Note that we can use this query to lift only `ps:contact` nodes for EnvolveLabs employees since those are currently the only `ps:contact` nodes in this data set. We would need to run a more specific query (such as `ps:contact:type=employee`)  if this data set also included contact information for other individuals. 

<u>**Lifts Example 2: Finding a limited number of nodes**</u>

In Lifts - Example 1, we found all the employees at EnvolveLabs by typing the name of the Form `ps:contact` that corresponds to the employee data model and lifting all `ps:contact` nodes. But, what if we wanted to look at an even larger set of nodes, for instance, all the domains seen in EnvolveLabs data?

As with Example 1, let’s start by typing the Form name associated with domains:

```css
inet:fqdn
```

This query brings back over 2,000 results. To see a more manageable set of the data, we can pipe the output of the query to the limit command, which will only return the specified numbers of results. Let’s run this query to see only 10 domains:

```css
inet:fqdn | limit 10
```

>Lift 10 employees. Enter “done” when you have finished. 

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

>What is the CEO’s name? (hint: lift by the title)


### Filter

Filter operations are performed on the output of a previous Storm operation, such as a lift or pivot. We use filter to downselect the set of nodes by either including or excluding nodes based on a set of criteria:

* `+` specifies an inclusion filter (include only the nodes that match the criteria)
* `-` specifies an exclusion filter (include all nodes except those that match the criteria)


<u>Filters Example 1: Inclusion Filtering</u>

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

Now, we’ve found all the emails sent by Stephanie. But, remember, we need to find only those messages about economics. We can add a filter based on the subject line of the email messages to find such emails. In this case, we’ll use an inclusion filter with a regular expression (regex) comparator to look for the term “economics” anywhere within the email’s Subject line:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" +:subject~="economics"
```

This leaves us with only one result.

<u>**Filters Example 2: Exclusion Filtering**</u>

Let’s look again at Stephanie’s emails. But this time, we want to ignore any email that might mention an opinion. For this, we’ll use an exclusion filter:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" -:subject~="opinions"
```

This leaves us 17 results.


### Pivot

Pivot operations are performed on the output of a previous Storm operation such as a lift or filter. Pivot operators are used to navigate from one set of nodes to another based on a specified relationship. These relationships often include shared property values.

We mentioned earlier that forms have properties, which are specific fields that hold information about the node modeled in Synapse. Each form has, at a minimum, a primary property, which is the name of the form and definition of the value for individual instances of that form, and which a user must populate in order to create the node within Synapse. For some forms, the primary property value may be a Globally Unique Identifier (GUID). For the `inet:email` node shown below, the primary property value for is "alexander_hall@envolvelabs.xyz":

```css
inet:email=alexander_hall@envolvelabs.xyz
```

Many forms also have secondary properties, which are optional fields capable of holding additional information, but which are not necessary for creating the node within Synapse. The `inet:email:message form`, for example, has multiple secondary properties, such as `:date`, `:body`, `:bytes`, `:to`, and `:from`, among others. We can pivot from the `inet:email=alexander_hall@envolvelabs.xyz` to all associated `inet:email:message` nodes that share the value "alexander_hall@envolvelabs.xyz". While "alexander_hall@envolvelabs.xyz" is the primary property value for the `inet:email` node, it may appear as one of several secondary property values (`:to`, `:from`, `:replyto`) for the associated `inet:email:message` nodes.

![image6](https://github.com/KC7-Foundation/kc7_data/assets/9474932/058bce1f-d96d-484a-b12c-e78bbc47fa43)

Pivoting from `inet:email=alexander_hall@envolvelabs.xyz` to associated `inet:email:message` nodes using Storm would look like the following (note the use of “->”, which is the pivot out operator):

```css
inet:email=alexander_hall@envolvelabs.xyz -> inet:email:message
```

However, it is also possible to lift `inet:email=alexander_hall@envolvelabs.xyz` and pivot out to all associated `inet:email:message` nodes without using Storm at all. We can do this by switching the Query Bar from Storm to Lookup mode, querying alexander_hall@envolvelabs.xyz, and then using the Explore button to pivot to connected nodes, including `inet:email:message nodes`: 

![explore_UI](https://github.com/KC7-Foundation/kc7_data/assets/9474932/75c4faec-ab56-4f13-a6f6-a616cc2005e7)


<u>**Pivot Out Example 1**</u>

Suppose you wanted to find all the links sent in the emails sent by Stephanie Clark. We can accomplish this using the pivot out operator

First, we lift to find emails sent by Stephanie:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" 
```

Then we pivot on those messages to find those that also include embedded links:

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

<u>**Pivot Out Example 2**</u>

In a few specific instances, nodes are connected by lightweight (light edges) representing a specific, directional relationship between the nodes. For example, a blog modeled as a media:news node that references multiple indicators will have a “refs” lightweight edge pointing to the nodes representing those referenced indicators. In this data set, lightweight edges appear between risk:alert nodes and nodes that the alerts reference. The query below shows a pivot from a risk:alert node to the file:bytes node that the alert references:

```css
risk:alert:detected="2023/11/13 15:51:25" -(refs)> file:bytes
```

## Section 2 Start Hunting!

You’ve finished your training and you’re ready to get to work protecting EnvolveLabs.

Work with your team to complete as many challenge questions from the remaining sections in the scoreboard as possible! The goal is to score as many points as you can. There are a lot of questions (the attackers have been busy), so you probably won’t be able to answer them all. Just do as many as you can!

As you answer the questions, we will take you on a journey exploring the data and discovering what actions the adversaries have taken. However, you should remember that this is only one of many paths you can take through the data. As you go, don’t forget to pay attention to the details along the way. What patterns do the attackers exhibit that could help you track them better? Do they like to use certain words, or themes? Or do they make mistakes? Keeping track of these patterns will help you build the full picture of what happened.

Use the provided Actor Preview document to keep track of what you know about the attacker. Building a good profile, timelining the attacker’s activity, and forming a list of indicators of compromise (IOCs) will help you keep track of the attacker. KC7 models some of the techniques used by these attackers from real world threat actors, so it may be a helpful resource for you in the future when you are investigating a real security incident.

Now, get out there and keep us safe! The whole company is counting on you. No pressure😊.

## Section 3: Clustering!

Now that we learned more about the Techniques, Tactics and the Procedures, used by the adversaries, we want to start documenting what we know so we can answer meaningful questions and build detections to block them. Instead of storing what we know about the adversaries as notes on a piece of paper, or just written words that someone else will need to re-interpret, we will label the relevant nodes using tags to keep track of our findings. 

Synapse gives you a structured way to record observations or assessments through the use of labels (tags) applied to data (nodes). Assessments represent conclusions based on the data available to you at the time. Tags are flexible and can be easily added, updated, or removed when assessments change, meaning that you can revise your analysis as needed when  new data becomes available.

For simplicity, we’ll nickname the first threat actor we are tracking “lightning.” We’ll use the tag  `#cno.threat.lightning` to label all nodes that correspond to this threat actor. You can apply tags to a node by right-clicking on the node in the Results pane of the Research Tool, and then selecting “Add Tags” from the context menu, as shown below:

![tag_add1](https://github.com/KC7-Foundation/kc7_data/assets/9474932/58002dc2-3556-497a-a621-d17c7c696d30)

Or, alternatively, from selecting the node and clicking the “Add Tags” button on the Details pane on the right hand side:

![tag_add2](https://github.com/KC7-Foundation/kc7_data/assets/9474932/c338c69f-baf1-4c44-84f1-7863c572a594)

Once you’ve applied the tag, you’ll see it appear in the Detail pane view for the tagged node. You can remove the tag by clicking on it in the Details pane and selecting “Remove full tag” from the menu:

![tag_remove](https://github.com/KC7-Foundation/kc7_data/assets/9474932/a7921c50-42b2-49c7-afea-ffa70aaad82b)


### References


