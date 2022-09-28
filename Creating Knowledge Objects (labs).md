# Creating Knowledge Objects (labs)

##
## Field Aliases
Field aliases give you a way to normalized data for multiple sources

You can assigned one or more aliases to any field and apply to fields from a lookup table

Normalizing user and username fields to a commong field name **Employee** would make correlation searching our data much easier

_Reference splunk Common Information Model (CIM) when creating field aliases_
##
***
## Tags
Tags are use to give fields functions and location lables 

To create a tag click on an event information link and then the action link for the field value pair we want to tag, and edit tag link will apear, click it and we get the "create tag" dialog box

Tags are case sensitive

You can search for a tag associated with a value on a specific field using a double colon followed by the field name and the tag name
tag::host=Production
##
***
## Calculated fields
The eval command is used to calculate and manipulate field values, the results of eval commands can be written to new fields or replace existing fields.

A calculated field can help wen performing repetitive, long or complex eval commands

**Convert bytes into megabytes**
| eval bandwith = round(sc_bytes/1024/1024,2) 

Without the calculated field
<img src=_resources/d66e93db85d10a273d8f4ce4f4428f85.png>

with calculated field "bandwith" ( bandwith=round(sc_bytes/1024/1024,2) ) 
![6a461d7e718683a8d365bf45f1746ec6.png](../_resources/6a461d7e718683a8d365bf45f1746ec6.png)

Calculated fields must be based on extracted fields

Output fields from a lookup table or fields generated from within a Search string are cannot be used
##
***
## Event Types
Event types allow the user to categorized events based on search terms

Event types help to simplify searches

To create a event type  Create a search 
<img src=_resources/2d78e39fa79d37d566dc6d94f8159306.png>

then Go to search and save as event type
<img src=_resources/7621a1af41d7c6b1cc20082216cf9b04.png>

Name the event and add tags, you can also pick a color and a priority for the event, all events with that color will have the same priority  
![ae884acd370a31df80eaca304cedbdea.png](../_resources/ae884acd370a31df80eaca304cedbdea.png)

Running a search with the value of an event type will return events that match the search terms used to create the event type
![acb5a39498ff22624e5fccc3d0837088.png](../_resources/acb5a39498ff22624e5fccc3d0837088.png)

The event type will show in the field sidebar

An event type can also be built in the event type builder

From an event information id we can select Built event type on the Event Action menu
![afd187b271f8381be24ac018bf4d948c.png](../_resources/afd187b271f8381be24ac018bf4d948c.png)

To manage event types we go to settings and then event types
We can set priorities to tell splunk if there is an event that fits in multiple event types, which event type takes precedence in the display order

**_Event Types vs Saved Reports_**

When to create event types vs when to use a saved report

**Event Types**
* Categorize events based on search strings
* Use tags to organize data
* "eventtype" field withing a search string
* Do not include a time range

**Saved Reports**
* Fixed search criteria 
* Time range & formatting needed
* Share with splunk users
* Add to dashboards
##
**
## Search Macros

* Macros are search strings or portions of search strings that can be use in multiple places withing splunk

* They are useful for frequent searches with complicated search symtax

* Macros allow you to stpre entire search strings including pipes and eval statements

* They are time range independing allowing the time range to be selected at search time 

* They can also pass aguments to the search
##
**Creating a Macro**

We start with a search summing all the sales by vendor and then passing those to an eval comman that formats the totals to display in US dollars amounts

![440527c5923bd7159d8b83b194c102c5.png](../_resources/440527c5923bd7159d8b83b194c102c5.png)

To save us from having to enter the eval command each time we can create a Macro

We go to settings, then advance search and then clicking the "add new" action for Search macros
![0f50aeea2ef3391eea92dc3318c9a777.png](../_resources/0f50aeea2ef3391eea92dc3318c9a777.png)

To test the macro by piping the search into the macro instead of the eval command we use earlier, to use the macro we surround the name of the macro with a back tick ` `` ` character, the backtick tell splunk this is a macro and to replace it with the search in the macro definition

![b754bd75a6e84d80774f4fe7df9b069a.png](../_resources/b754bd75a6e84d80774f4fe7df9b069a.png)

##
**Adding Arguments**

The goal of macros is to make them reusable as posible

_Dynamic macros_

settings--> advance search --> search macros ---- Here we can set up our macros and set permisions

To add an argument we clone the macro, the sample below shows that we are require 2 different arguments (name = convertUSD(2))
We have to add the name of the arguments on the order that the command will be passed to the macro
![56a948d8e6e90ce964774bc286a2d829.png](../_resources/56a948d8e6e90ce964774bc286a2d829.png)

##
**Validating Macro Arguments**

Splunk allows you to validate the arguments sent using an eval or boolean expression 

making a requirement that the "cmd" argument passed to the macro is either "fieldformat" or "eval" and if not send an error to the user

We enter a boolean as a validation expression and a validation message to send the user if the boolean is false
![925ddc64511ef66436a99ee55624e0b6.png](../_resources/925ddc64511ef66436a99ee55624e0b6.png)

Now if we search, we pass a command name as an argument
![4ec76e66ed37b94da44bed28e52bb1a3.png](../_resources/4ec76e66ed37b94da44bed28e52bb1a3.png)
With this search we are converting with eval command so the results  do not sort numerically
By passing a fieldformat command to the macro we can have numerically sort the results
![7370e607eff07a8d2aaa24a9b806c1f2.png](../_resources/7370e607eff07a8d2aaa24a9b806c1f2.png)

If we pass a nonvalid command to the macro we get a searchpass error with the error text appended
![537c6a70cd5b33abea168cbe6791dab0.png](../_resources/537c6a70cd5b33abea168cbe6791dab0.png)

Splunk has a search expansion tool that allows you to preview your search by extending the entire search without running it

**Search expansion**
Splunk has a search expansion tool that allow you to expand your search without running it

With the search in the search bar use command shift E (or control shift E in microsoft or linux)
![8ec705581c08a8987ea91202bb92f4a2.png](../_resources/8ec705581c08a8987ea91202bb92f4a2.png)
The search expansion window will open and you will be able to see the search as it will be submitted.
This comes handy if you want to copy a fragment of the expanded search and use it in another search

##
***
## Creating workflow Action

**Workflow Actions**

Helps us create links withing events that interact with external resources or narrow down our search.

They use the HTTP GET or POST methods to pass information to external sources or can pass information back to splunk to perform a secondary search

Search workflow actions launch secondary searches that use specific field values from an event

GET workflow actions create typical HTML links to do things like perform Google searches on specific values or run domain name queries against external WHOIS databases. 

POST workflow actions generate an HTTP POST request to a specified URI. This action type enables you to do things like creating entries in external issue management systems using a set of relevant field values. 

Which function is used to send field values externally in Workflow Actions? POST

**Workflow GET method example**

In the linux_secure sourcetype we can find external IPs trying to SSH into the server as root with incorrect passwords
![547c7beffde9c082eb20a70adc5bf382.png](../_resources/547c7beffde9c082eb20a70adc5bf382.png)

The security team can run a WHOIS on the IPs to find out to whom the IP is registered, and where the traffic is coming from
![9070f9e147817672bf9647aa7cab011a.png](../_resources/9070f9e147817672bf9647aa7cab011a.png)

Normally they would copy and paste the IP into the WHOIS tool of choice but with workflow actions we can create a link that does the work for them

![67ba79f57a36c2192c33a42ddf6771f2.png](../_resources/67ba79f57a36c2192c33a42ddf6771f2.png)

Splunk has a prebuilt flow action, when we clink on it from an event's "Event Action"dropdown menu we are taking to the domaintools.com website with the IP from our event filled in

![2f4702e0abd4b362511c32d1e7a2c3c0.png](../_resources/2f4702e0abd4b362511c32d1e7a2c3c0.png)

To create a workflow action, we go to settings, then fields, and click "add new" for the workflow action type

The label will display in the UI where we launch the action, if we want to use the current field value in the lable we enter the field name enclosed in dollard signs `$src_ip$`

Then we enter the field to apply the action to, in this case it will only be src_ip

We can also choose wich event types to apply the workflow action to, we wanted available to all so we leave it empty
![a8d4002c100fd5e5b7abad34d3192914.png](../_resources/a8d4002c100fd5e5b7abad34d3192914.png)

The show action in dropdown show us where to show the workflow action: in the event manu, fields menu or both

The "action type" dropdown will select if this action is a link or a search, we will use link for this example

We would be using the domaintools.com whois service with our workflow action, we know it requires an IP variable to passed in an HTTP GET method
![1a44967450f531637626f3ef02526dd4.png](../_resources/1a44967450f531637626f3ef02526dd4.png)

In the URL field we enter the link for the tool and the variable of the field `htpp://whoid.domaintools.com/$src_ip$`
Every service is different and is up to you to match the URI to match the requirements if the field values we are adding to the URI included data that needed to be escaped we can add an exclamation point inside the first dollard sign `htpp://whoid.domaintools.com/$!src_ip$` but we do not needed for this example

We want the action link to open in a new browser window so we choose new window in the "open link in" dialog
We set the "link method" to GET
![b2da3440bde732320fc31162a39a779c.png](../_resources/b2da3440bde732320fc31162a39a779c.png)

To test our workflow action we can re-run our search
![b4082349f6bc4f3df5be2c2ce27af3ba.png](../_resources/b4082349f6bc4f3df5be2c2ce27af3ba.png)

click the info botton for one of the IPs we want to check, and then select "Get whois for (ip number)" from the even actions dropdowsn

![5a55484eaa6eb26e0dd48df97b1b6382.png](../_resources/5a55484eaa6eb26e0dd48df97b1b6382.png)

We are taken to the domaine Whois site with the IP variable filled in

![4b3548fca839abdf3670bb433ac181e0.png](../_resources/4b3548fca839abdf3670bb433ac181e0.png)

**Workflow POST method example**

We can also use an HTTP POST method in a workflow action

The buttercupgame development team has created a bug tracking system

![442697c25dee750ba00cce0efcb77108.png](../_resources/442697c25dee750ba00cce0efcb77108.png)

The tool accepts form input to create a support ticket. Using a workflow action with a link method of POST we can send data directly from our event tracking tool.

To create one , we follow the asme steps we did in the GET example: Picking an application,  creating a name, making a label, choosing which field and event type to limit
![7a04fcec24ca7b8c1d355b9314b515a7.png](../_resources/7a04fcec24ca7b8c1d355b9314b515a7.png)

Where to show (show action in) and selecting an action type of "link", for the URI we will enter the address we want the data posted to
We choose to have the link open in a new window and select POST in our link method

![8da1efd00fd24508551d53fed43e686d.png](../_resources/8da1efd00fd24508551d53fed43e686d.png)

Next we need to create arguments to send the POST data, you can use any value data from an event by using the field name wrapped in dollard signs ($status$ error $host$)

We create arguments for summary, priority, time occured,  the enviroment it happened and full details of the event

![45962d7249000640702168d30b9e0d85.png](../_resources/45962d7249000640702168d30b9e0d85.png)

Now if we run a search with server errors

![72759c1dc454134426c6e77470662829.png](../_resources/72759c1dc454134426c6e77470662829.png)


And select create tickets from an event

![c96b366ec9098848ce7ff88c6b698de3.png](../_resources/c96b366ec9098848ce7ff88c6b698de3.png)

The bug tracker opens displaying a newly created ticket

![9f72998de121310840e27c3f0b7919d8.png](../_resources/9f72998de121310840e27c3f0b7919d8.png)

Workflow actions can come very handy when you want to send pre-filled forms or communicate with an API

A workflow action can also be use to launch a search

We would like to take our failed password example from before and add an action where you can search for other events where the IP might appear
![299df8e884c7e5d29415539bd98b46bb.png](../_resources/299df8e884c7e5d29415539bd98b46bb.png)

We follow the same steps as out GET and POST examples
![5d0189a5058badccef5df6100ff9e748.png](../_resources/5d0189a5058badccef5df6100ff9e748.png)

Until we get to the accion type fields, here we select "search", which brings up the Search configuration 

We enter the search string to use when the action is launched, since we want to see every event including the IP, we can just enter the IP field as a variable

We select wich app to run the search, we can choose to have the search open in a specific view and we can select of the search should open in a new browser window
![a9c8415d614cc6e18e6d4c2c990dfc41.png](../_resources/a9c8415d614cc6e18e6d4c2c990dfc41.png)

THere are time limit inputs, but we will be using the same range as the search that created the field listing

![742c4e3444af2f3b207544baeeb94c41.png](../_resources/742c4e3444af2f3b207544baeeb94c41.png)

When we run our search, there is a find other events for (ip address) workflow actions in the event actions

![e03e22f277a670ca6cecc4397d2102d7.png](../_resources/e03e22f277a670ca6cecc4397d2102d7.png)

When we click on it, we can see all the events that include the IP address

![8110fede0c4637634aed13d025330fb4.png](../_resources/8110fede0c4637634aed13d025330fb4.png)

## 
***
## Search Time Operations Sequence

When a search is run, Splunk process knowledge objects in a set order of operations and applies them to events returned by the search

**Field Extractions**

At search time, Splunk will first perform:
* Field Extractions followed by 
* Operations for Field Aliases
* Calculated Fields
* Lookups
* Event Types
* Tags

This process is linear so each operation can reference fields derived from opeations that precede it, but they cannot reference operations that follow it. 
**For example:**
If a calculated field is use in a search, its underlying eval command can reference an aliasesd field name 
![ff1c759571eded359f5e7f4085c4c1cc.png](../_resources/ff1c759571eded359f5e7f4085c4c1cc.png)

But if a field is from a lookup, since lookups are after calculated fields in the sequence, using it in a calculated field will return an error

![71e20e97c516954ad10107317df8d212.png](../_resources/71e20e97c516954ad10107317df8d212.png)

This calculations are always apply in the same order

![0083125e0d6dc30ab82f99ed00d570f9.png](../_resources/0083125e0d6dc30ab82f99ed00d570f9.png)

So it is important to keep their sequence in mind when creating knowledge objects and using them in searches

_Operation configuration documentation:_ https://docs.splunk.com/Documentation/Splunk/latest/Knowledge/Searchtimeoperationssequence

***
# Questions

1. Field aliases are applied after _________ and before ________ . Select all that apply.
a.  lookups, field extractions
b. tags, field extractions
##

2. Select all knowledge objects.
a. lookups *
b. field aliases *
c. users
d. workflow actions *
