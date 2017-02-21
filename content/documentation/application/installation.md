+++
title = "Installation of EcoSystem and Project Setup"
date = "2017-02-19T10:40:34+01:00"

+++

The EcoSystem command-line application is written in Golang, which means it compiles to an executable binary on any platform.  At this stage, we do not offer precompiled executables, so you'll have to download the source code and compile it on your machine.  Thankfully, Go makes this trivial.  Remember - you won't need to know Go to use EcoSystem (although it helps if you want to make a custom build).

### Install Go

I highly recommend you follow the instructions in the [Go Getting Started guide](https://golang.org/doc/install).  Installing Go isn't difficult and should only take you a few minutes.

If you're new to Go and this is a fresh installation, take some time to understand the Go path and folder structure.  It's important that your *go/bin* directory is in your PATH so that Go binaries can be run from anywhere.  For Mac/Linux users, this is usually just a case of adding the following to your */etc/profile* (for a system-wide installation) or *$HOME/.profile*:

```
export PATH=$PATH:/usr/local/go/bin
```

If you have any trouble installing Go, read through the [official docs](https://golang.org/doc/) or Google 'install go [your operating system]' for help.

### Install EcoSystem

Once you have Go installed, run `go get github.com/ecosystemsoftware/ecosystem`.  This will download the source code and dependencies and compile the binary executable into your *go/bin* directory.

Test that you can run EcoSystem from anywhere by running `ecosystem --help` from the command line.  You should see the help instructions for EcoSystem.

### Create a project folder

EcoSystem relies on a particular folder structure relative to where it is being run from, so the first step in starting an EcoSystem project is to create a new folder called `my-project` or similar.

### Create a configuration file

From your root project folder, run `ecosystem new configfile` to create a template *config.json* which is needed by the rest of the commands.  See the [configuration documentation](/documentation/application/configuration/) to learn about configuration.  The first step is to correctly define the parameters for your database connection.

### Initialise EcoSystem

Once you have correctly input the database connection paramaters into your *config.json*, run `ecosystem init`.  This command initialises both the folder structure in your project, as well as the built-in database tables and permissions.  See the [commands documentation](/documentation/application/commands) for more details.
