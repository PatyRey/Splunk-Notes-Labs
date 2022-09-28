# Intro to Dashboards - Fundamentals 2

## Splunk Dashboard Frames

Splunk provide two frameworks for creating dashboars:
1. Classic Dashboard
* Source code: simple XML
* Layout: row and column

##

2. Dashboard studio
* Source code: JSON
* Layouts: absolute and grid
* Layering visualization
* More visualization images, icons, shapes, and text boxes

##

**Managing views**
* Scoped to your app context
* Open, clone, move, delete
* Set sharing Permissions

Dashboards also known as views are scoped to your appication context, they exist in the application they where created in

However there is a management screen that allows you to actually change various attributes of the dashboard or view, such as moving them to different application context, deleting them, making copies of them, and opening them up and sharing them

If you have a view that you need to migrate from one application to another you need to go to view management screen:
* go to settings
* Usier interfaces
* view 
* Select the application in which the particular dashboard exist
* Set the owner to yourself
* Then you have the options to open | clone | move | delete

Splunk provides a mechanism for converting classic dashboards into dashboard studio

(***) <--- elipsis


##
***
## Dashboard Definition
When a dashboard is created the underlying JSON-formatted dashboard definition is automatically generated

Tjis definition includes five sections:
* Data Sources
* Visualization
* Defaults
* Inputs
* Layout
* As well as the dashboard description ( "this week's numbers") and title ("Web Sales")

THe order of this sections will change depending on the on how the dashboard was originally created.

For example, a dashboard created from a search will begin with data sources section which contains the search queries, along with options for those queries and dashboard unique ID and type

If the dashboard is created in the dashboard page, its definition will begin with the **visualizations** section which contains the visualization, unique ID, the visualization type, **data sources**, and related options. 
Global **defaults** for the dashboard are contained in the defaults section, the **input** section contains a unique ID, and input stanzas for any inputs in the dashboard, while the **layout** section contains a list of those inputs as well as canvas size and position and size information for the visualizations

##
***
## Making a Plan

Many operations in Splunk, such as configuring permissions and uploading files will require an administrator, custome stylesheets and interactivity may require you to call upon a javascript developer or UX designer, security experts can ensure that the dashboard complies with any security requirements and end users can help you understand how your data is actually being used day to day, your stakeholders can help you determine the complexity needed for your dashboards by answering wuestions such as :
Who are your end users and what's their skill level?
what metrix are critical to their individual roles? 
what's the time span for your data and how freaquently should be refreshed? 
what type of visualization should be requered? 
and how they will be laid out?


##
***
## Creating a Prototype

The planning stage of building a dashboard start with the stakeholders, identifying metrics and timeframes
An important part of the planning stage is sketching out a wireframe, think about the panels you will need, the type of vizualizations that may best suit your data, the data you want behind them and how should be arranged

Buildt a prototype of your dashboard in splunk using basic searches. selecting visualizations and arraging panels to align with your wireframe.

The next step in building the dashboard is to add interactivity by adding buttons and form inputs 

Consider the ways you want ot allow users to manipulate your data

And the types of inputs that are best suited to those needs

Planning which type of inpust you'll need at this stage helps you to know how to set tokens in future stages
Once everyone is happy with the prototype is time to focus on the performance

This is where we'll make sure our searches are as efficient as possible, ensure our tokens are correctly passing variables and accelerate teports and data models



##
***
## Troubleshooting

Steps to investigate when something goes wrong

The most important step when trying to diagnose a dashboard issue is to go to the source, if the panel is powerded by an inline search, copy the search string and run it manually, to make sure you are getting the data expected
![164c9d429cd2f5fff1953dac57d1a644.png](../_resources/164c9d429cd2f5fff1953dac57d1a644.png)

Check the macro searchs if you have it

If you have double checked the rest of your search string and can't have the problem, isolate the searches and ensure that they are working as expected

For **Visualizations** Is alway good to retrace your steps
check your search history
also you can also use the history command in the search app  `| history ` to see your previous searches stored as events

If you have a panel power by a report view the report to ensure it's functioniing properly, make sure to take note of schedule and acceleration setitngs.
If the dashboard uses token inout, it's important to make sure those tokens are functioning as expected 

the search job inspector allows you to see what's happening behind the scenes of a search, once a search is completed, you can see how many resultas the search has returned, how many events have been scan and how long the search took to execute

![028dbaaa27ba896cc3b0b8788623bf59.png](../_resources/028dbaaa27ba896cc3b0b8788623bf59.png)


##
***
## Creating a Dashboard


##
***
## Visual vs Source Editor


##
***
## Layout settings


##
***
## Working with Visualizations


##
***
## Adding a drilldown
