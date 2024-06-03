# Welcome to EnvolveLabs!

🥳 Today is your first day as a Junior Security Operations Center (SOC) analyst. Your job is to safeguard EnvolveLabs, a leading research hub in Ukraine, and its dedicated team from potential cyber threats, especially during these challenging times of the Russian invasion.

![2](https://github.com/KC7-Foundation/kc7_data/assets/9474932/db6fffb7-6f81-4fc1-ada7-ab0af4700732)


EnvolveLabs, situated in Ukraine's vibrant tech landscape, is a nexus of groundbreaking research in the intertwined fields of energy and artificial intelligence (AI). This research institute is harnessing renewable energy sources, all while employing advanced AI algorithms to optimize energy production, distribution, and consumption. Beyond just energy, they're leveraging AI to predict energy demands, streamline grid operations, and even forecast environmental impacts.

However, in the wake of the Russian invasion, their cutting-edge research and strategic location have inadvertently made EnvolveLabs a prime target for cyber threats and espionage. Your mission, if you choose to accept it, is to investigate attacks against EvolveLabs so they can help keep the lights on for everyone in the country.

## Objectives

🧠 **By the end of your first day on the job, you should be able to:**
	
- Pivot on data using Storm and Synapse, learning how to perform Lift, Filter, and Pivot operations. 
- Learn how to label nodes to form an analytical layer. 
- Investigate cyber activity in logs including: email, web traffic, and server logs.
- Use multiple techniques to track the activity of APTs (Advanced Persistent Threats).
- Use third-party datasets to discover things about your attackers.


🚀 The attackers have gotten a head start, so let's not waste any more time... **let's get to work!**


## Getting Set Up in Synapse

A few things you’ll need to do first:

1. Request a Synapse Enterprise demo instance at: [https://vertex.link/request-a-demo](https://vertex.link/request-a-demo). You’ll then receive an email from `Optic <signup@vertex.link>` with a link to create a password and log in to your demo instance.

2. Once you’ve logged in, check the green **Top Bar** to verify that you are in the **Research Tool** (you should see "Research" in the left hand corner).

	![research](https://github.com/KC7-Foundation/kc7_data/assets/55701249/4032d0df-981c-4dcb-ad74-5372d14b716d)

4. Next, use the **Workspace Selector** dropdown menu in the Top Bar to change from "default" to the *KC7 Workspace*.

	![kc7_workspace](https://github.com/KC7-Foundation/kc7_data/assets/55701249/a568405b-61b6-4514-a8a1-962f2e8e9846)

5. Use the **View Selector** to select the *KC7-EnvolveLabs View* from the dropdown menu. This is where the data for today’s exercise lives. 

	![kc7_view](https://github.com/KC7-Foundation/kc7_data/assets/55701249/1410f844-5c85-4fbd-ba11-37591a5eda19)

6. Now, create a scratchspace to work out of by clicking the **Fork** icon to the right of the **View Selector**: 

	![fork](https://github.com/KC7-Foundation/kc7_data/assets/55701249/6b802086-5e60-4f48-99dd-a3f18892f065)

7. This will open a pop-up window where you can name your "scratchspace" then click **Fork** to create it.

   	![fork_window](https://github.com/KC7-Foundation/kc7_data/assets/55701249/300cc51c-af25-41f6-b077-a6e36c00e82f)

   You’ll see the name of your scratchspace replace the *KC7-EnvolveLabs* view in the Top Bar:

   ![top_bar](https://github.com/KC7-Foundation/kc7_data/assets/55701249/c6bc90e9-a47a-4d47-8a38-ba3ea4bd9dfe)

8. Now that you've set the Workspace and view, double check that the **Query Bar**, shown below, is set to Storm mode:
   
	![storm_mode](https://github.com/KC7-Foundation/kc7_data/assets/55701249/00f34dcb-bf8d-4ac0-9502-c675bec9038a)

	You’ll be using this **Query Bar** to run our queries for today’s exercises. 

9. Now you need to install a Power-Up. Use the Tool Bar on the left of the UI, navigate to the **Power-Ups Tool** by clicking on the lightning bolt icon, then search for the *Synapse-Alienvault* Power-Up under the *Available* tab and click the **Add** button to install it on your Synapse instance:
	
 	![power-up_install](https://github.com/KC7-Foundation/kc7_data/assets/9474932/4121af76-4f42-4ae2-811a-76b427eefc23)

10. Once you’ve installed the Power-Up, you'll need to set your AlienVault API key (you can sign up for a free key here: https://otx.alienvault.com/). Use the Tool Bar on the left to navigate to the **Console Tool** by clicking on the icon, shown below:
    
	<img width="238" alt="image" src="https://github.com/KC7-Foundation/kc7_data/assets/9474932/9c46b2a2-6ffc-4266-97b1-9deeffcb9c7a">

11. Type the following command into the **Query Bar** and hit Enter to set your Alienvault API key:

```js
alienvault.setup.apikey <apikey>
```

10. Once you’ve done that, use the magnifying glass icon at the top of the left-hand toolbar to return to the **Research Tool**. 

	![research_tool](https://github.com/KC7-Foundation/kc7_data/assets/55701249/17202138-a677-4061-94e4-e52d7e86f600)

<br>

## Section 1: The Walkthrough

<br>

### First Look at the Data…

EnvolveLabs maintains a log of various events taking place within the company. This data is gathered from multiple sources such as network sensors, firewalls, and endpoint products. This information is pivotal for detecting and countering malicious activities. You can access this data in Synapse using the following forms:

| Form Used           | Description                                                                                          |
|---------------------|------------------------------------------------------------------------------------------------------|
| `ps:contact`        | Information about employees hired at the company and their corresponding assets                      |
| `inet:email`              | An email address                                 |
| `inet:email:message`| Emails sent and received by employees at the company                                                 |
| `inet:email:message:link` | A URL/link embedded in an email message          |
| `it:host`           | Information about hosts assigned to the company’s employees                                          |                                                                                     |
| `inet:fqdn`               | A fully qualified domain name                    |
| `inet:ipv4`               | An IPv4 address                                  |
| `inet:url`                | A URL                                            |
| `inet:http:request` | Web browsing activity to and from the Internet, indicating websites visited by our employees and external users browsing our websites |
| `inet:dns:a`        | IP-domain resolutions provided by a third-party vendor                                               |
| `it:logon`          | Successful and failed logins to devices on the company network, including logins to the company’s mail server |
| `it:exec:file:add`  | Data about one of the employee-assigned host machines adding a file to a filesystem                  |
| `it:exec:proc`      | Data regarding processes created on employee’s devices                                               |
| `risk:alert`        | Security alerts from employees’ devices and the company’s email security system                      |
| `file:base`               | A filename                                       |
| `ou:jobtitle`             | A job title                                      |

<br>

	
>**ℹ️ A Note on Forms, Properties, and Nodes:**
>
>It can be helpful to think of a [Form](https://synapse.docs.vertex.link/en/latest/synapse/userguides/data_model.html#form) as a template for capturing a certain kind of information in Synapse. For example, EnvolveLabs uses the ps:contact form to capture individual contact information about the company’s employees. Forms have [properties](https://synapse.docs.vertex.link/en/latest/synapse/userguides/data_model.html#property), which are specific fields that provide additional information. Properties found on the `ps:contact` form, for example, include `:name`, `:email`, `:title`, and `:type`, among others. 
>We can use a form to capture information within Synapse - filling out the template as it were - at which point we refer to that information as a node. The image below shows the `ps:contact` node for EnvolveLabs employee Cecil Bryant:
>
>![cecil bryant employee](https://github.com/KC7-Foundation/kc7_data/assets/9474932/93b611dc-73e7-4906-b848-d635d0083eb3)
>
>The properties displayed are `:name`, `:title`, `:user`, `:email`, and `:type`, the last of which categorizes this entry as an employee. 
>
>You can review all currently available Forms within Synapse in the **Data Model Explorer**. The **Data Model Explorer** is an index of all Forms in Synapse and provides information such as, what the Form is used to capture, which properties are available, and how the Form relates to others within Synapse’s data model. You can access the **Data Model Explorer** via the **Help Tool**, as shown below:
>
>![data_model_explorer](https://github.com/KC7-Foundation/kc7_data/assets/9474932/a6456f9e-db81-45d6-846c-575de195d9a1)

<br>

## Storm Operations

There are three basic operations we’ll use to query the company’s logs using Storm: **Lift, Filter, and Pivot**. We can build everything on top of these basics. 

In some cases, we can use the Synapse UI to answer the question rather than running a Storm query. We’ll point out how to do that when possible.

<br>

### Lift

Every Storm query begins with a [Lift operation](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_lift.html) to retrieve a set of nodes based on specific criteria. Here are some examples:

1. "Show me all the email addresses"
```css
inet:email
```
<br>

2. "Show me the node representing the email address daniele_rael@envolvelabs[.]xyz"

```css
inet:email=daniele_rael@envolvelabs.xyz
```
<br>

3. "Show me all email messages sent to the email address daniele_rael@envolvelabs[.]xyz"
```css
inet:email:message:to=daniele_rael@envolvelabs.xyz
```
<br>  

<u>**Standard and Extended Comparisons**</u>

Lift operations can do more than just show us all the nodes in Synapse. Often, we want to use a Storm query to find only specific nodes that meet a given set of criteria. Suppose we want to find out more information about someone named Stephanie Clark.

We’ll be looking at `ps:contact` nodes once again in this query, but this time, we’ll focus specifically on the node’s `:name` property. Using the = comparator, we can find the `ps:contact` node where the `:name` property value exactly matches the name "Stephanie Clark":

```css
ps:contact:name="stephanie clark"
```
<br>

If, instead, we want to find all `ps:contact` nodes for people whose name starts with "Stephanie", we could use the prefix comparator (`^=`) to lift these nodes:

```css
ps:contact:name^=stephanie
```

<br>
There are several other comparison operators that we may find useful going forward. These include:
<br>

| Operator | Lift by                                   |
|----------|-------------------------------------------|
| =        | Exact value                               |
| ^=       | Prefix                                    |
| ~=       | Regular expression (Regex)                |
| @=       | Time or Interval                          |
| >        | Value is greater than                     |
| <        | Value is less than                        |
| >=       | Value greater than or equal to            |
| <=       | Value less than or equal to               |


<br>

Here are some example Lift operations using [Standard Common Operators](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_lift.html#lifts-using-standard-comparison-operators): 

<br>

| Description                                                       | Command                                            |
|-------------------------------------------------------------------|----------------------------------------------------|
| Lift email messages sent from the email address, “`gregorysmith@aol.com`” | `inet:email:message:from=gregorysmith@aol.com`     |
| Lift email messages whose subject starts with the word, “`Important`” | `inet:email:message:subject^=Important`            |
| Lift alerts containing the process “`waitfor.exe`” in the description | `risk:alert:desc~=waitfor.exe`                     |
| Lift HTTP requests that took place between January 1, 2023 (“`20230101`”) and January 31, 2023. | `inet:http:request:time@=(20230101,20230201)`   `inet:http:request:time@=(20230101, “+31 days”)`   |
| Lift email messages sent after November 8, 2023 (“`20231108`”)      | `inet:email:message:date>20231108`                |
| Lift email messages sent before November 8, 2023 (“`20231108`”)     | `inet:email:message:date<20231109`                |
| Lift process execution events that occurred on or after November 15, 2023 (“`20231115`”) | `it:exec:proc:time>=20231115`                    |
| Lift process execution events that occurred on or before November 15, 2023 (“`20231115`”) | `it:exec:proc:time<=20231115`                    |

<br> 
 
### Pivot

<br>

A [Pivot operation](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_pivot.html#) is performed on the output of a previous Storm operation such as a Lift, and is used to navigate from one set of nodes to another based on a shared property value. 

We mentioned earlier that Forms have properties which are specific fields that hold information about the node. Each Form has, at a minimum, a [primary property](https://synapse.docs.vertex.link/en/latest/synapse/userguides/data_model.html#primary-property), which is the name of the Form and unique value representing each individual node of that Form. A user must populate the primary property to create the node within Synapse. For some Forms, the primary property value may be a Globally Unique Identifier (GUID). For the `inet:email` node shown below, the primary property value for is "alexander_hall@envolvelabs[.]xyz":

```css
inet:email=alexander_hall@envolvelabs.xyz
```
<br>

Many forms also have [secondary properties](https://synapse.docs.vertex.link/en/latest/synapse/userguides/data_model.html#secondary-property), which are optional fields capable of holding additional information, but which are not necessary for creating the node within Synapse. The `inet:email:message form`, for example, has multiple secondary properties, such as `:date`, `:body`, `:bytes`, `:to`, and `:from`, among others. We can pivot from the `inet:email=alexander_hall@envolvelabs.xyz` to all associated `inet:email:message` nodes that share the value "alexander_hall@envolvelabs[.]xyz". While "alexander_hall@envolvelabs[.]xyz" is the primary property value for the `inet:email` node, it may appear as one of several secondary property values (`:to`, `:from`, `:replyto`) for the associated `inet:email:message` nodes.
<br>

![image6](https://github.com/KC7-Foundation/kc7_data/assets/9474932/058bce1f-d96d-484a-b12c-e78bbc47fa43)

<br>
We can pivot among nodes using either the Synapse UI or the Storm query language. 

<br>

<u>**Pivoting through the UI**</u>
<br>

If we choose to pivot through the UI, we have the option of using the [Explore](https://synapse.docs.vertex.link/projects/optic/en/latest/user_interface/userguides/quick_tour.html#explore-button-breadcrumbs) button, which will allow us to pivot to all nodes connected to our original node (or nodes). In the example below, we’ve lifted `inet:email=alexander_hall@envolvelabs.xyz` and use the Explore button to view connected nodes, which include `inet:user`, `inet:fqdn`, `ps:contact`, and `inet:email:message` nodes:
<br>

![explore](https://github.com/KC7-Foundation/kc7_data/assets/55701249/b18ef463-b116-4fb2-8daa-26c4f8e0e90e)
<br>

Using the Explore button will also result in a ["breadcrumbs"](https://synapse.docs.vertex.link/projects/optic/en/latest/user_interface/userguides/quick_tour.html#explore-button-breadcrumbs) trail, shown at the top of the Results panel, which we can use to navigate back to our prior set of nodes if needed:
<br>

![breadcrumbs](https://github.com/KC7-Foundation/kc7_data/assets/55701249/a32b9b82-ec1c-45e6-871e-b20302cb0e06)
<br>

Since the Explore button will navigate out to all related nodes (as opposed only navigating from the `inet:email` node to related `inet:email:message` nodes, etc), using the Explore button can be a helpful way to easily view connected data and "see what’s out there" before attempting a more targeted pivot.

The other way to pivot through the UI is by using the pivot option in the Context menu, which users can access through right-clicking on a selected node (or nodes) in the Results panel. Whereas the Explore button will navigate out to all connected nodes, the pivot option in the Context menu will provide several pivot options. In the example below, we use the pivot menu to pivot from the `inet:email` address to any related `inet:user` nodes:
<br>

![pivot_menu](https://github.com/KC7-Foundation/kc7_data/assets/55701249/4629cd57-d9e3-410c-beda-2545731bed78)

<br>

<u>**Pivoting through Storm**</u>
<br>

We have the ability to be more precise and specific when using the Storm query language for pivoting, as we can specify which shared property values we’d like to use. The majority of pivots will be pivoting out, and thus will involve the use of the [Pivot Out Operator](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_pivot.html#pivot-out-operator) (`->`). Pivoting from `inet:email=alexander_hall@envolvelabs.xyz` to associated `inet:email:message` nodes using Storm would therefore look like:

```css
inet:email=alexander_hall@envolvelabs.xyz -> inet:email:message
```
<br>
In this query, Synapse understands that we are using the shared property value "alexander_hall@envolvelabs[.]xyz" to navigate from the `inet:email` node to the related `inet:email:message` nodes that contain that email address as a `:to`, `:from`, or `:replyto` property value. However, in some instances, we’ll have to be more explicit in our navigation and identify for Synapse which property value we are using in our navigation. For example, in the following query, we have lifted a `ps:contact` node where the `:name` property is equal to "Mariya Bondarenko":

```css
ps:contact:name="Mariya Bondarenko"
```
From here, we’d like to pivot to the assigned host (captured as an `it:host`) associated with Mariya Bondarenko, which we can do as the `ps:contact` node’s primary property (a GUID) is the value of the `:operator` property for the `it:host` node:

```css
ps:contact:name="Mariya Bondarenko" -> it:host
```
However, now we want to navigate from the `it:host` node to the `inet:http:request` nodes capturing the HTTP requests from that host. To make that pivot, we’ll need to switch from using the `ps:contact` value to the IPv4 property value shared between the `it:host` and `inet:http:request` nodes. Since the IPv4 is a secondary property value for both nodes, we’ll need to specify that we are using the `:ipv4` property value from the `it:host` node to pivot to the `:client:ipv4` property on the `inet:http:request` nodes. Our query will look like this: 

```css
ps:contact:name="Mariya Bondarenko" -> it:host :ipv4 -> inet:http:request:client:ipv4
```
This type of query, in which we specifically identify the properties on which we are pivoting, is known as [explicit syntax](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_pivot.html#implicit-pivot-syntax) and is the most reliable way to pivot since we are clarifying for Synapse which properties we are using to navigate from one set of nodes to another. 

<br>

>**ℹ️ A Note on Storm Commands:**
><br>
><br>
>In some instances, we’ll want to include a [Storm command](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_cmd.html) in our query to help us count the number of resulting nodes, remove duplicates, or lift a limited number of results, etc. Here are some Storm commands that may come in handy:
><br>
>
>| Command | Description                                                         |
>|---------|---------------------------------------------------------------------|
>| count   | Iterate through query results, and print the resulting number of nodes to the Console Tool |
>| limit   | Limit the number of nodes generated by the query to a specific number of results |
>| min     | Lift the resulting node with the lowest value for the specified property |
>| max     | Lift the resulting node with the greatest value for the specified property |
>| uniq    | Remove duplicate nodes from the query results                       |
>
><br>
>When incorporating a Storm command into a query, we’ll need to use the pipe (`|`) character to pipe the inbound nodes to the command. A query to lift the most recently received email message modeled in our Synapse instance will look like this:
>
>```
>inet:email:message | max :date
>```
>
>If we want to continue our query after the Storm command, we’ll need to use the pipe character once more between the Storm command and the rest of the query. As an example, if we take the query above to lift the most recently received email message, and pivot to any embedded URLs sent within the email, our query will look like this:
>
>```
>inet:email:message | max :date | -> inet:email:message:link
>```

<br>

<u>**Traverse**</u>
<br>

In the majority of cases, we’ll navigate among nodes by pivoting across shared property values. However, in a few specific instances, nodes are connected by [lightweight (light) edges](https://synapse.docs.vertex.link/en/latest/synapse/userguides/data_model.html#lightweight-light-edge) representing a specific, directional relationship between the nodes. Navigating among these relationships is known as traversing the light edge.

For example, a blog modeled as a `media:news` node that references multiple indicators will have a "refs" lightweight edge pointing to the nodes representing those referenced indicators. In this dataset, lightweight edges appear between `risk:alert` nodes and nodes that the alerts reference. The query below shows a pivot from a `risk:alert` node to the `file:bytes` node that the alert references:

```css
risk:alert:detected="2023/11/13 15:51:25" -(refs)> file:bytes
```
<br>

### Filter
<br>

[Filter operations](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_filter.html#simple-filters) are performed on the output of a previous Storm operation, such as a lift or pivot. We use filter to downselect the set of nodes by either including or excluding nodes based on a set of criteria:

* `+` specifies an _inclusion filter_ (include **only** the nodes that match the criteria)
* `-` specifies an _exclusion filter_ (include all nodes **except** those that match the criteria)

<br>

<u>**Inclusion Filter**</u>
<br>

In the following Storm query, we’ve lifted all emails sent by the email address "stephanie_clark@envolvelabs[.]xyz":

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" 
```

However, we are only interested in a subset of these results - namely those email messages where the subject line mentions the word "economics." We can alter our existing lift operation to add an inclusion filter that makes use of a [Regular Expression (regex) Comparator](https://synapse.docs.vertex.link/en/latest/synapse/userguides/storm_ref_lift.html#lift-by-regular-expression) to look for messages that include the term "economics" anywhere within the subject line:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" +:subject~="economics"
```

This results in one `inet:email:message` node.
<br>

<u>**Exclusion Filter**</u>
<br>
In contrast, we may want to remove a subset of nodes from our overall result set, which would require the use of an exclusion filter. For example, we may decide that we want to see all email messages from stephanie_clark@envolvelabs[.]xyz except for those that mention "economics" in the subject line:

```css
inet:email:message:from="stephanie_clark@envolvelabs.xyz" -:subject~="economics"
```
This leaves us with 20 results.

<br>

### Applying Tags
<br>

Finally, during your investigation, you may find it helpful to keep track your findings by applying a [tag](https://synapse.docs.vertex.link/en/latest/synapse/userguides/analytical_model.html#) or label to certain node(s). 

Synapse gives you a structured way to record observations or assessments through the use of labels (tags) applied to data (nodes). Assessments represent conclusions based on the data available to you at the time. Tags are flexible and can be easily added, updated, or removed when assessments change, meaning that you can revise your analysis as needed when  new data becomes available.

For simplicity, we’ll nickname the first threat actor we are tracking “lightning.” We’ll use the tag  `#cno.threat.lightning` to label all nodes that correspond to this threat actor. You can apply tags to a node by right-clicking on the node in the **Results** pane of the **Research Tool**, and then selecting _Add Tags_ from the context menu, as shown below:

![tag_add1 (1)](https://github.com/KC7-Foundation/kc7_data/assets/9474932/e2efea45-4a75-452b-82dd-8d1f6b5f116d)
<br>

Alternatively you can select the node and click the _Add Tags_ button on the **Details** pane on the right-hand side:

![tag_add2 (1)](https://github.com/KC7-Foundation/kc7_data/assets/9474932/f840e27a-094c-456a-8a47-7fefa80cb9b6)
<br>

Once you’ve applied the tag, you’ll see it appear in the **Details** pane view for the tagged node. You can remove the tag by clicking on it in the **Details** pane and selecting _Remove full tag_:

![tag_remove (1)](https://github.com/KC7-Foundation/kc7_data/assets/9474932/b8c65166-3ab7-45cb-a4b2-764dbc2d2b17)
<br>


## Section 2 Start Hunting!
<br>

You’ve finished your training and you’re ready to get to work protecting EnvolveLabs.

Work with your team to complete as many challenge questions from the remaining sections in the scoreboard as possible! The goal is to score as many points as you can. There are a lot of questions (the attackers have been busy), so you probably won’t be able to answer them all. Just do as many as you can!

As you answer the questions, we will take you on a journey exploring the data and discovering what actions the adversaries have taken. However, you should remember that this is only one of many paths you can take through the data. As you go, don’t forget to pay attention to the details along the way. What patterns do the attackers exhibit that could help you track them better? Do they like to use certain words, or themes? Or do they make mistakes? Keeping track of these patterns will help you build the full picture of what happened.

Use the provided Actor Preview document to keep track of what you know about the attacker. Building a good profile, timelining the attacker’s activity, and forming a list of indicators of compromise (IOCs) will help you keep track of the attacker. KC7 models some of the techniques used by these attackers from real world threat actors, so it may be a helpful resource for you in the future when you are investigating a real security incident.

Now, get out there and keep us safe! The whole company is counting on you. No pressure😊.

![cat typing gif](https://media.tenor.com/CrWHpzxIZYEAAAAC/cat-typing-gif.gif)





