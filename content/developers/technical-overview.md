+++
date = "2017-02-02T17:01:00+01:00"
title = "Overview of EcoSystem for Developers"

+++

## Here's a quick technical summary, highlighting the main features of EcoSystem.

- EcoSystem is not a single, monolithic piece of software - it is a web/API server written in Golang and an administration interface created using Polymer.
- Flexibility comes at a price.  EcoSystem does not give you everything on a plate.  Most significantly, you have to create your own database.  This need not be particularly complex, but it can be if required.

### Database

- A Postgres database needs to be manually created and customised for each EcoSystem installation.
- There are no limits or requirements on what data structures you create at the database level.
- The levels of EcoSystem above the DB are 'thin' or 'dumb'.  They don't know anything about business data or logic.
- This requires you to put all business logic, including constraints, calculations and data operations in the database.
- In order to achieve this, you should liberally use Postgres's powerful views, triggers and functions.
- The only EcoSystem specific quirk that you need to know is in the naming of tables and views.  We use the underscore ("_") to link views to their corresponding table.  So, for example, a view built on a table called 'customers', might be called 'customers_fullinfo'.  We'll explain in this in more detail shortly.
- The server switches roles according to the user before contacting the database, so you'll use Postgres's roles and permissions for security.

### Server

-  The EcoSystem server is written in Golang for ease of development, speed and ease of deployment.
-  The server ships as an executable binary and does not require any configuration beyond telling it where your database is.
-  If you'd like to build a custom version of the server in order to include some special functionality, you can include it in your project as a Go package as well.
-  The base functionality of the server is achieved with very little code, this is because the server is dumb.  It does not process data from the database in any way, hence there is no ORM.  In fact, the server requests JSON directly from the database, using Postgres's super speedy JSON output capability.
-  The server, then, acts almost as a straight "pass through", interpreting requests and serving database data in the form of JSON or templated HTML.
-  Table naming is key - it's the only way the server knows how to interact with the database. An API request to something like /api/customers_fullinfo will look for a a view called 'customers_fullinfo'.  If that view doesn't exist, you'll get a 404.
-  The primary role of the server is as a JSON API through which you interact with your data structures and logic in the database.  This allows you to build an unlimited array of applications.
-  A secondary role of the server is to serve a templated HTML website, for installations that require a 'traditional' HTML website.
-  Templates are standard, simple Golang templates and are linked logically to database views by name: 'customers_fullinfo-list.html' is how you want the list of customers to be displayed in HTML.  Don't want that list to appear on the website? Don't create a template.
-  HTML delivered by the server can be used as full pages and/or HTML partials, so you can build hybrid websites using any combination of full-page server-side-rendered HTML, server-side-rendered HTML partials (with AJAX, [Intercooler JS](https://www.intercoolerjs.org) is highly recommended for this) and/or client-side rendered JSON from the API.
-  The server also offers 'auxiliary' services like authentication, image resizing etc.  As EcoSystem develops, we'll develop these auxiliary functions in accordance with the needs of users.

### Administration interface

- There is nothing special about the EcoSystem interface - it is simply a Polymer app that connects to the EcoSystem API
- Out of the box, the admin interface will not show anything.  You need to tell it what you need to see.  You do this by configuring 'views'.
- Fundamentally, a view is a screen consisting of fields and lists.
- A file called views.json lists all the views you create and their relationship to database tables.  For example, a view called 'customersDefault' might be created to show the 'customers_fullinfo' view from the database.
- To spec out a view called 'customersDefault' you just create a 'customersDefualt.json' configuration file and use a powerful set of attributes to specify what information you want to see.
- For example, the attribute 'data-columns' is a list of the fields to display. The attribute 'allow-sort' will show clickable icons in the header to allow data to be sorted.  The attribute 'allow-bulk-select' will bring up checkboxes next to each record, creating the possibility of bulk actions.
- Views are composable - you can have lists embedded in views of records and so on.
- Fields can be made editable in both list and drilldown views.  Ability to edit can be controlled at view and individual field level.
- Custom functions can be written and linked to both per-line and bulk actions in order to implement rich user interface functionality, like adding items to orders, bulk discounting or fulfilling.

***A full technical overview and detailed documentation are on their way - please sit tight or contact us if you have any questions that need answering right now***
