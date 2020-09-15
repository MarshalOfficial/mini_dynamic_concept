# Light Dynamic Concept!

In this concept we will use a **Dynamic** mechanism to create and rendering fully functional web or mobile apps. It will be hard at first to create database, API and front-end infrastructure, and then be easy to maintain and develop your product with less coding.


# Basic Objects

They are multiple basic object that we use to processing user interface elements and behavior of the forms.


## Reports

Every list-view that shows data by rows, we call it report here, and will have multiple columns with some properties that will say below.


## Actions

Every form that show some input on it and have button to submit a request, we call it an action here.

## Menu

A hierarchic tree base menu that will shown as admin panel, which item of menu will be used for a report or an action.

## Metadata
The core data information about objects, we name it metadata or meta. Each object has its own meta information, for example a menu item meta information will show title, type and etc, an action meta information will show what are input parameters of action and what and how many submit button does it need. don't worry we will explain more and detail about it later in below.



# Scenario

The Back-end will response to front-end some metadata that consist of menus, front-end will populate and hold menus metadata, then shown the menus to the user, each menu will be for an object of an action or report, when user click on an action menu, front-end will dynamically generate a form with inputs and button that metadata said for this action, or when user click on a report menu, front-end dynamically generate report page in row and column base list-view with some optional filter in header of the page.
With this platform we can do most of functional software needs dynamically and the front-end code will be with the least changes and with back-end we will generate multiple forms and report that users need.
There is no matter you use which language, framework and stack for you'r database, back-end rest API and front-end, for me these days i am using MS SQL server database engine, python FastAPI framework for the rest API and angular or react for the front.
	
## Menu Metadata
A simple menu metadata shown below that we have two nodes in root of menu and one child for first element, as we mentioned before each menu element must be action or report and in data property back-end populated each item metadata for front-end, so front know if element 2 clicked it must generate an action form for adding a new user.
```json
{
   "Menu":[
      {
         "ID":"1",
         "Key":"btnUsers",
         "Title":"LocalizedString",
         "Type":"Report",
         "ParentID":"null",
         "Meta":"Cotains a Json for a Report Metadata that front will generate it when user click"
      },
      {
         "ID":"2",
         "Key":"btnAddUser",
         "Title":"LocalizedString",
         "Type":"Action",
         "ParentID":"1",
         "Meta":"Cotains a Json for an action Metadata that front will generate it when user click"
      },
      {
         "ID":"3",
         "Key":"btnSaleReport",
         "Title":"LocalizedString",
         "Type":"Report",
         "ParentID":"null",
         "Meta":"Cotains a Json for a Report Metadata that front will generate it when user click"
      }
   ]
}
```
## Action  Metadata
For example an action metadata for update a user will be like below, it says to front-end: we need a form with one submit button and 6 input, 2 string type input in first row, one date and one checkbox in the second row and  last row will have a select and drop down input type.
```json
{
   "parameters":[
      {
         "ID":"FirstName",
         "DataType":"string",
         "Title":"Localizationed",
         "Row":"1",
         "Col":"1"
      },
      {
         "ID":"LastName",
         "DataType":"string",
         "Title":"Localizationed",
         "Row":"1",
         "Col":"2"
      },
      {
         "ID":"BirthDate",
         "DataType":"date",
         "Title":"Localizationed",
         "Row":"2",
         "Col":"1"
      },
      {
         "ID":"IsActive",
         "DataType":"bool",
         "Title":"Localizationed",
         "Row":"2",
         "Col":"2"
      },
      {
         "ID":"Gender",
         "DataType":"select",
         "Title":"Localizationed",
         "Row":"3",
         "Col":"1",
         "DataSource":[
            {
               "ID":"1",
               "Title":"Male"
            },
            {
               "ID":"2",
               "Title":"Female"
            },
            {
               "ID":"3",
               "Title":"Bisexual"
            }
         ]
      },
      {
         "ID":"BloodGroup",
         "DataType":"dropdown",
         "Title":"Localizationed",
         "Row":"3",
         "Col":"2",
         "DataSourceProcedure":"BloodGroupGet"
      }
   ],
   "buttons":[
      {
         "Title":"Localizationed",
         "ProcedureName":"UserUpdate"
      }
   ]
}
```
DATA BINDING: If there was any input data passed to an action via json and there was any property with same name of action parameters inside the action, the front-end must bind the values to pair input. 
DATA TYPES : and also there is a data type mapping between database and front-end:
|db & back-end|front-end                          |
|----------------|-------------------------------|
|varchar|string|
|int/bigint|number|
|decimal|decimal|
|bit|bool|
|date|date|
|datetime|datetime|
|automatically prepare in data-source via json from back-end|select|
|will prepare via back-end calling with proc name|drop down|

## Report Metadata
```json
{
   "properties":{
      "LoadOnOpen":"true",
      "ProcName":"UsersGet"
   },
   "filters":[
      {
         "ID":"FirstName",
         "Title":"LocalizedString",
         "DataType":"string",
         "defaultValue":"",
         "index":"0"
      },
      {
         "ID":"LastName",
         "Title":"LocalizedString",
         "DataType":"string",
         "defaultValue":"",
         "index":"1"
      },
      {
         "ID":"Gender",
         "Title":"LocalizedString",
         "DataType":"select",
         "defaultValue":"",
         "index":"2",
         "DataSource":[
            {
               "ID":"1",
               "Title":"Male"
            },
            {
               "ID":"2",
               "Title":"Female"
            },
            {
               "ID":"3",
               "Title":"Bisexual"
            }
         ]
      }
   ],
   "columns":[
      {
         "ID":"FirstName",
         "DataType":"string",
         "Title":"Localizationed",
         "index":"0"
      },
      {
         "ID":"LastName",
         "DataType":"string",
         "Title":"Localizationed",
         "index":"1"
      },
      {
         "ID":"BirthDate",
         "DataType":"date",
         "Title":"Localizationed",
         "index":"2"
      },
      {
         "ID":"IsActive",
         "DataType":"bool",
         "Title":"Localizationed",
         "index":"3"
      },
      {
         "ID":"Gender",
         "DataType":"select",
         "Title":"Localizationed",
         "index":"4"
      },
      {
         "ID":"BloodGroup",
         "DataType":"dropdown",
         "Title":"Localizationed",
         "index":"5"
      },
      {
         "ID":"CreateUserName",
         "DataType":"string",
         "Title":"Localizationed",
         "index":"6"
      },
      {
         "ID":"CreateDateFa",
         "DataType":"string",
         "Title":"Localizationed",
         "index":"7"
      }
   ],
   "HeaderActions":[
      {
         "ID":"AddUser",
         "Title":"LocalizedString",
         "Meta":"contains a json of the action metadata"
      }
   ],
   "InRowActions":[
      {
         "ID":"EditUser",
         "Title":"LocalizedString",
         "Meta":"contains a json of the action metadata"
      },
      {
         "ID":"DeleteUser",
         "Title":"LocalizedString",
         "Meta":"contains a json of the action metadata"
      }
   ]
}
```
Policies:
1- InRowActions must pass current row of list view data to the action that user clicked via Data parameter of actions.

## Publish a File

You can publish your file by opening the **Publish** sub-menu and by clicking **Publish to**. For some locations, you can choose between the following formats:

- Markdown: publish the Markdown text on a website that can interpret it (**GitHub** for instance),
- HTML: publish the file converted to HTML via a Handlebars template (on a blog for example).

## Update a publication

After publishing, StackEdit keeps your file linked to that publication which makes it easy for you to re-publish it. Once you have modified your file and you want to update your publication, click on the **Publish now** button in the navigation bar.

> **Note:** The **Publish now** button is disabled if your file has not been published yet.

## Manage file publication

Since one file can be published to multiple locations, you can list and manage publish locations by clicking **File publication** in the **Publish** sub-menu. This allows you to list and remove publication locations that are linked to your file.


# Markdown extensions

StackEdit extends the standard Markdown syntax by adding extra **Markdown extensions**, providing you with some nice features.

> **ProTip:** You can disable any **Markdown extension** in the **File properties** dialog.


## SmartyPants

SmartyPants converts ASCII punctuation characters into "smart" typographic punctuation HTML entities. For example:

|                |ASCII                          |HTML                         |
|----------------|-------------------------------|-----------------------------|
|Single backticks|`'Isn't this fun?'`            |'Isn't this fun?'            |
|Quotes          |`"Isn't this fun?"`            |"Isn't this fun?"            |
|Dashes          |`-- is en-dash, --- is em-dash`|-- is en-dash, --- is em-dash|


## KaTeX

You can render LaTeX mathematical expressions using [KaTeX](https://khan.github.io/KaTeX/):

The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$

> You can find more information about **LaTeX** mathematical expressions [here](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference).


## UML diagrams

You can render UML diagrams using [Mermaid](https://mermaidjs.github.io/). For example, this will produce a sequence diagram:

```mermaid
sequenceDiagram
Alice ->> Bob: Hello Bob, how are you?
Bob-->>John: How about you John?
Bob--x Alice: I am good thanks!
Bob-x John: I am good thanks!
Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

Bob-->Alice: Checking with John...
Alice->John: Yes... John, how are you?
```

And this will produce a flow chart:

```mermaid
graph LR
A[Square Rect] -- Link text --> B((Circle))
A --> C(Round Rect)
B --> D{Rhombus}
C --> D
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNTExODIzNjAsOTYwOTI2NTcsOTkzNz
MyMzAwLDk0NTY5MzE4MywtMTg0NzMxNjEwOCwtMTkzOTkzOTMy
LDY0NTE3OTI1MywxODc0OTA4Njg2LC0xNDE2NzA2MzM2LDcwMz
c1MDk0NiwxNTQ0OTE1MjMyLDQzMzM1MzcwNSwzMzIyODcyMCwt
MzgzOTU5NTI2LC0xNzU1OTE2MjIyLC0yMDI0Mzc2NDQyLDExOD
U0NzYyNjUsMzE1NDIwMTEyLC0zMzI0NTUzNjNdfQ==
-->