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

To start with, fetch the source code and dependencies and compile into an executable which will go into your $GOPATH/bin directory.  That's just `go get github.com/ecosystemsofware/ecosystem`.  Assuming your [$GOPATH/bin is part of your PATH](https://golang.org/doc/install), you should now be able to run 'ecosystem' from anywhere on the command line.

### Step 2:  Create a database

Log into your Postgres server and create a new database. Call it *testdb* - that way you won't have to make any changes to the default database configuration.

### Step 3: Create a project folder

Make a new folder from which to run the server and `cd` into it:

### Step 4: Create a configuration file

Just type `ecosystem` to get going.  Since you don't have a defualt *config,json*, EcoSystem will generate one for you.

### Step 5: Configure the database connection

If you're working locally, the defaults will probably just work out-of-the-box.  Otherwise, open *config.json* and edit the database connection parameters.

### Step 6: Initialise EcoSystem

EcoSystem needs to create a number of built-in tables, roles, permissions and functions, as well as a few folders, so just type `ecosystem init` to have it do that for you.

### Step 7: Create an admin user

Set yourself up as an admin user with full permissions by typing `ecosystem new user [your@email.com] --admin`

### Step 8: Download and install a bundle

The easiest way to get started with EcoSystem is by installing an existing bundle.  Bundles are just folders that group together everything EcoSystem needs: database code, sample data, website templates, admin panel configurations etc.  Once you get familiar with EcoSystem, you'll create your own bundles, but for now, let's clone a simple demo bundle and install it with demo data:

```
$ cd bundles
$ git clone git@github.com:ecosystemsoftware/eco_bundle_dogshelter.git
$ cd ..
$ ecosystem install eco_bundle_dogshelter --demodata
```

### Step 9: Download the EcoSystem admin panel app

From your main project folder, clone the EcoSystem admin panel repository by typing `git clone git@github.com:ecosystemsoftware/ecosystem-admin.git`

### Step 10: Run the server

Run the server in 'demo' mode (disabling authorisation) with `ecosystem serve -s=secret -d`