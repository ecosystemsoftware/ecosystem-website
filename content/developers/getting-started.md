+++
date = "2017-02-03T09:57:13+01:00"
title = "Getting Started"
+++

This is a step-by-step guide which should get you going with EcoSystem in around 2 minutes.  It deliberately glosses over a lot of detail.  Use this as a starting point and delve deeper into the documentation when you are ready.

*Note: we aren't yet distributing official binaries of the EcoSystem application, so you'll need to have Go installed to build it yourself.  You won't, however, need to know how to program in Go to use EcoSystem.*

### Prerequisites

1. Go installed and environment variables correctly configured.  You should make sure your [$GOPATH/bin is part of your PATH](https://golang.org/doc/install) so that installed Go packages can be run from anywhere.
2. A Postgres database server (local or remote) to which you can connect.
3. NodeJS and Bower installed and correctly configured (to install Polyme admin panel dependencies).

### Step 1: Install EcoSystem

Fetch the source code and dependencies and compile into an executable which will go into your $GOPATH/bin directory:

```
$ go get github.com/ecosystemsofware/ecosystem
```

Assuming your [$GOPATH/bin is part of your PATH](https://golang.org/doc/install), you should now be able to run 'ecosystem' from anywhere on the command line.

### Step 2:  Create a database

Log into your Postgres server and create a new database.  I'll assume you'll call it `test`, but you don't have to.

### Step 3: Create a project folder

Make a new folder from which to run the server and cd into it:

```
$ mkdir test
$ cd test
```

### Step 4: Create a configuration file

Generate a default config.json file which we'll use to set up the database connection:

```
$ ecosystem new configfile
```

### Step 5: Configure the database connection

Open `config.json` and edit the database connection parameters so EcoSystem can connect to the `test` database you created.  If you're working locally, you'll probably only have to change the database name and disable SSL mode.

### Step 6: Initialise EcoSystem

EcoSystem needs to create a number of built in tables, roles, permissions and functions, as well as a few folders:

```
$ ecosystem init
```
If all goes well, you'll see messages confirming successful initialisation.

### Step 7: Create an admin user

Set yourself up as an admin user with full permissions:

```
$ ecosystem new user [your@email.com] --admin
```

### Step 8: Download and install a bundle

The easiest way to get started with EcoSystem is by installing an existing bundle.  Bundles are just folders that group together everything EcoSystem needs: database code, sample data, website templates, admin panel configurations etc.  Once you get familiar with EcoSystem, you'll create your own bundles, but for now, let's clone a simple demo bundle and install it with demo data:

```
$ cd bundles
$ git clone git@github.com:ecosystemsoftware/eco_bundle_dogshelter.git
$ cd ..
$ ecosystem install eco_bundle_dogshelter --demodata
```

There's nothing magic going on under the hood here, we simply created a new folder in bundles called 'eco_bundle_dogshelter' which will be the name of the bundle, and copied in some files.  We then ran the database code in `install.sql` to set up the bundle's data and logic.  Finally we ran `demodata.sql` to install the bundle's demo data.  Everything else, like website template code and admin panel config code, lives in the bundle folder and the EcoSystem server knows to find it there.

### Step 9: Download the EcoSystem admin panel app

From your main project folder, clone the EcoSystem admin panel and install Polymer dependencies:

```
$ git clone git@github.com:ecosystemsoftware/ecosystem-admin.git
$ cd ecosystem-admin
$ bower install
$ cd..
```

### Step 10: Set up the bundle imports

*Note:  We working on a way of automating this step - until now, it's manual I'm afraid.*

The bundle contains various setting files for the admin panel, for them to take effect, you'll need the admin panel to import them.  The procedure is as follows:

1.  Open the file `bundles/eco_bundle_dogshelter/admin-panel/bundle-imports.html`
2.  Copy all the lines that start `<link rel="import"...`
3.  Open the file `ecosystem-admin/src/eco-bundle-imports.html`
4.  Paste the lines previously copied.

### Step 10: Run the server

Run the server in 'demo' mode (disabling authorisation).  You need to supply a 'secret' which is used by the server for encryption.  Here I use 'secret', but you can use anything.  Do this from the main project directory.

```
$ ecosystem serve -s=secret -d
```