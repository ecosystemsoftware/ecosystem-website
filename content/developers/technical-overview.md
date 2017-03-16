+++
date = "2017-02-02T17:01:00+01:00"
title = "Overview of EcoSystem for Developers"

+++

## Here's a quick technical summary, highlighting the main features of EcoSystem.

### General Philosophy

- Simplicity: keep it simple.  Wherever possible, choose simple architecture that is easy to reason about and learn.
- Ease-of-use: EcoSystem would often be an alternative to BaaS (backend-as-a-service), so we try to keep the barrier to entry low.
- Modularity: "Bundles" provide a simple way to encapsulate, organise, contribute, share and reuse data schemas and logic specifications.
- Extensibility: Basic, essential functionality in the core package and standard build with option to extend with Go packages and create custom builds.

### Database

- A Postgres database needs to be manually created and customised for each EcoSystem installation.
- There are no limits or requirements on what data structures you create at the database level.
- The levels of EcoSystem above the DB are 'thin' or 'dumb'.  They don't know anything about business data or logic.
- This requires you to put all business logic, including constraints, calculations and data operations in the database.
- In order to achieve this, you should liberally use Postgres's powerful views, triggers and functions.
- Database schemas are used to organise data and logic.
- The only EcoSystem specific quirk that you need to know is in the naming of tables and views.  We use the underscore ("_") to link views to their corresponding table.  So, for example, a view built on a table called 'customers', might be called 'customers_fullinfo'.  We'll explain in this in more detail shortly.
- The server switches roles according to the user before contacting the database, so you'll use Postgres's roles and permissions for security.
- Schema and table names translate directly to API endpoints: *store.products* would live at the endpoint */api/store/products*

### Server

-  The EcoSystem server is written in Golang for ease of development, speed and ease of deployment.
-  The server ships as an executable binary and requires only minimal configuration.  This is acheived with a `config.json` which EcoSystem can generate automatically for you.
-  If you'd like to build a custom version of the server in order to include some special functionality, you can include EcoSystem's packages as dependencies in your Go project.
-  The base functionality of the server is achieved with very little code, this is because the server is dumb.  It does not process data from the database in any way, hence there is no ORM.  In fact, the server requests JSON directly from the database, using Postgres's super speedy JSON output capability.
-  The server, then, acts almost as a straight "pass through", interpreting requests and serving database data ias JSON.
-  Table naming is key - it's the only way the server knows how to interact with the database. An API request to /api/store/products will look for a a table/view called *prodcts* in the schema *store*.  If that table/view doesn't exist, you'll get a 404.
-  The primary role of the server is as the JSON API through which you interact with your data structures and logic in the database.  This allows you to build an unlimited array of applications.
-  The server also includes some auxilliary "services", like authorisation, authentication, image resizing etc.  These are simple Go packages and can be added to or removed easily.  As EcoSystem matures, we aim to build the library of available packages to cover as many typical server needs as possible.

### Bundles

-  A "bundle" is EcoSystem jargon.
-  The simplest definition of a bundle is a folder containing SQL files with data and logic specifications (and optional demo data) for a particular domain.
-  Bundles can be easily installed, uninstalled and reinstalled.
-  Bunldes are a development convenience.  Organise a large project into bundles and manipulate as needed.
-  Bundles correspond directly to database schemas.  Installing a bundle (folder) named 'ecommerce' will create a database schema 'ecommerce', which will in turn create an API route 'ecommerce'.
-  As EcoSystem grows, bundles, being just folders, will be used to group together all resources relating to a particular domain, e.g., HTML templates for a website server or stattic site generator, demo images, administration panel configuration files etc. 
