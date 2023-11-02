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
	
- ✅ **Pivot on data using Storm and Synapse:** Learn how to lift, filter, and pivot.
- ✅ **Labeling nodes:** Understand how to label nodes to form an analytical layer.
- ✅ **Data analysis:** Use multiple data sets to answer targeted questions.
- ✅ **Cyber activity investigation:** Examine logs, including email, web traffic, and server logs.
- ✅ **Tracking APTs:** Employ multiple techniques to track the activity of Advanced Persistent Threats.
- ✅ **Utilize third-party data sets:** Discover insights about your attackers.
- ✅ **Recommendations:** Advise on actions a company can take to protect themselves.

🚀 The attackers have gotten a head start, so let's not waste any more time... **let's get to work!**

## Section 1: The Walkthrough

EnvolveLabs collects log data about the activity our employees perform on the organization's network. These security audit logs are stored in Synapse - a versatile central intelligence and analysis system developed by the Vertex Project. You will use the Storm query language to pivot through EnvolveLabs’ telemetry, and label clusters of malicious activity you find along the way. By analyzing these logs, you can help us determine whether we’re being targeted by malicious actors and help us stop the adversaries!

### Getting Set Up in Synapse

A few things you’ll need to do first:

1. If you have not already received a Synapse demo instance, you can request one [here](https://vertex.link/request-a-demo).

2. Once you’ve requested a Synapse demo instance, you will receive an email from “Optic <signup@vertex.link>” with a link to your Synapse demo instance. Click the link to create a password and log into your Synapse instance.

3. Once you have logged in, you should be in the Research Tool. Verify this by checking the green Top Bar at the top of the UI. It should show “Research” in the left-hand corner. Alternatively, navigate to the Research Tool by

4. Use the Workspace Selector dropdown menu in the Top Bar to change from “default” to the “KC7 Workspace”. This will be the first dropdown menu to the right of where it says “Research”.

5. After selecting the KC7 Workspace, move to the dropdown menu to the right of the Workspace selector, and select the KC7 Cyberwarcon 2023 view. This is where the data for today’s exercise is located. The Top Bar of your UI should now have a specific appearance (a visual representation or description would typically follow here).

6. Next, we’re going to create a scratchspace to work out of. To do this, click the fork icon to the right of the View Selector:

	This will give you a pop-up window where you can name your scratchspace, then click “Fork” to create it. You’ll see the name of your scratchspace replace the “KC7 Cyberwarcon 2023” view in the View Selector.

7. Double check that the Query Bar, shown below, is set to Storm mode:

	We’ll be using this Query Bar to run our queries for today’s exercises. 


8. Use the Toolbar on the left of the UI to navigate to the Power-Ups Tool by clicking on the lightning bolt icon. Once in the Power-Ups Tool, use the search bar to search for the Synapse-Alienvault Power-Up in the “Available” tab. Click the “Add” button to install it on your Synapse Instance:

	[image here]

	Do the same for the Synapse-Maxmind Power-Up.


9. Once you’ve installed the Power-Ups, use the Toolbar on the left of the UI to navigate to the Console Tool by clicking on the Console Tool icon, shown below: 

10. Type the following command into the Query Bar and hit Enter to set your Alienvault API key: 

11. Once you’ve done that, use the magnifying glass icon at the top of the left hand toolbar to return to the Research Tool. 

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

| Form           | Description                                                                                          |
|---------------------|------------------------------------------------------------------------------------------------------|
| `inet:fqdn`         | A fully qualified domain name                                                                        |
| `inet:ipv4`         | An IPv4 address 


## STORM 101

There are three basic operations we’ll use to query the company’s logs using Storm: Lift, Filter, and Pivot. We can build everything on top of these basics. 

### Lift

Lift operations retrieve a set of nodes based on specified criteria. Let’s try a few lifts to get comfortable using Storm and working with the EnvolveLabs datasets.

**Lifts - Example 1: Let’s find all the employees at EnvolveLabs**

To look at all the employees in the EnvolveLabs data, we can use a lift operation. We need to start by typing the Form for the employee object. Here, the form for employees is `ps:contact`.
Enter the following in the Query Bar:

```css
ps:contact
```

We’ll find there are 1,513 results. Each result is a node that represents a unique employee at the company, with more detail about each employee - such as name, email address, and title - captured as a node property.

Question: How many employees are in the company?

_Lifts Example 2: Finding a limited number of nodes_

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

_Lifts Example 3: Standard and Extended Comparisons_

Lift can do more than just show us all nodes in the hypergraph. Often, we want to use a Storm query to find only specific nodes that meet a given set of criteria. Suppose we wanted to find more information about Envolve Labs employee Stephanie Clark.

We’ll be looking at ps:contact nodes for this query, but this time, we’ll focus specifically on the node’s :name property. Using the = comparator, we can find the exact node that belongs to Stephanie Clark:

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


_Filters Example 1: Inclusion Filtering_

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

Now, we’ve found all emails sent by Stephanie. But, remember, we need to find only those messages about economics. We can add a filter based on the subject line of the email messages to find such emails. In this case, we’ll use an inclusion filter with a regular expression (regex) comparator to look for the term “economics” anywhere within the email’s Subject line:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" +:subject~="economics"
```

This leaves us with only one result.

_Filters Example 2: Exclusion Filtering_

Let’s look again at Stephanie’s emails. But this time, we want to ignore any email that might mention an opinion. For this, we’ll use an exclusion filter:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" -:subject~="opinions"
```

This leaves us 17 results.

### Pivot

Pivot operations are performed on the output of a previous Storm operation such as a lift or filter. Pivot operators are used to navigate from one set of nodes to another based on a specified relationship.

Pivot operations can help us join information together from multiple sources throughout the course of our investigation.

We’ll primarily use the pivot out operator, which is denoted by the arrow symbol `->`

_Pivot Out Example 1_

Suppose you wanted to find all the links sent in the emails sent by Stephanie Clark. We can accomplish this using the pivot out operator

First, we lift to find emails sent by Stephanie:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" 
```

Then we pivot on those messages to find those that also include embedded links:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" -> inet:email:message:link 
```

Then, we pivot from the inet:email:message:link nodes representing those messages to the inet:url nodes representing just the URLs themselves.

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" -> inet:email:message:link -> inet:url
```

Of course, there could have been multiple links that were sent by Stephanie. We can view the unique links by piping the existing query to the `uniq` command. 

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" -> inet:email:message:link -> inet:url | uniq
```

## Section 2 Start Hunting!

You’ve finished your training and you’re ready to get to work protecting EnvolveLabs.

Work with your team to complete as many challenge questions from the remaining sections in the scoreboard as possible! The goal is to score as many points as you can. There are a lot of questions (the attackers have been busy), so you probably won’t be able to answer them all. Just do as many as you can!

As you answer the questions, we will take you on a journey exploring the data and discovering what actions the adversaries have taken. However, you should remember that this is only one of many paths you can take through the data. As you go, don’t forget to pay attention to the details along the way. What patterns do the attackers exhibit that could help you track them better? Do they like to use certain words, themes? Or do they make mistakes? Keeping track of these patterns will help you build the full picture of what happened.

Use the provided Actor Preview document to keep track of what you know about the attacker. Building a good profile, timelining the attacker’s activity, and forming a list of indicators of compromise (IOCs) will help you keep track of the attacker. KC7 models some of the techniques used by these attackers from real world threat actors, so it may be a helpful resource for you in the future when you are investigating a real security incident.

Now, get out there and keep us safe! The whole company is counting on you. No pressure😊.

## Section 3: Clustering!





